title: Android MediaPlayer
id: 1174
categories:
  - Android
date: 2013-09-09 19:29:50
tags:
---

**MediaPlayer包含的接口:**

**MediaPlayer.OnBufferingUpdateListener 	**
Interface definition of a callback to be invoked indicating buffering status of a media resource being streamed over the network. 
**MediaPlayer.OnCompletionListener **	
Interface definition for a callback to be invoked when playback of a media source has completed. 
**MediaPlayer.OnErrorListener **	
Interface definition of a callback to be invoked when there has been an error during an asynchronous operation (other errors will throw exceptions at method call time). 
**MediaPlayer.OnInfoListener **	
Interface definition of a callback to be invoked to communicate some info and/or warning about the media or its playback. 
**MediaPlayer.OnPreparedListener 	**
Interface definition for a callback to be invoked when the media source is ready for playback. 
**MediaPlayer.OnSeekCompleteListener **	
Interface definition of a callback to be invoked indicating the completion of a seek operation. 
**MediaPlayer.OnTimedTextListener **	
Interface definition of a callback to be invoked when a timed text is available for display. 
**MediaPlayer.OnVideoSizeChangedListener 	**
Interface definition of a callback to be invoked when the video size is first known or updated  
<!--more-->
使用MediaPlayer确保允许使用相关权限
Internet Permission:
```java
<uses-permission android:name="android.permission.INTERNET" />
```
Wake Lock Permission:
Uses the MediaPlayer.setScreenOnWhilePlaying() or MediaPlayer.setWakeMode() methods, you must request this permission. 
```java
<uses-permission android:name="android.permission.WAKE_LOCK" />
```

播放保存在应用的res/raw/directory目录的本地资源例子:
```java
MediaPlayer mediaPlayer = MediaPlayer.create(context, R.raw.sound_file_1);
mediaPlayer.start(); // no need to call prepare(); create() does that for you
```

播放本地URL资源:
```java
Uri myUri = ....; // initialize Uri here
MediaPlayer mediaPlayer = new MediaPlayer();
mediaPlayer.setAudioStreamType(AudioManager.STREAM_MUSIC);
mediaPlayer.setDataSource(getApplicationContext(), myUri);
mediaPlayer.prepare();
mediaPlayer.start();
```

通过HTTP流播放远程URL资源:
```java
String url = "http://........"; // your URL here
MediaPlayer mediaPlayer = new MediaPlayer();
mediaPlayer.setAudioStreamType(AudioManager.STREAM_MUSIC);
mediaPlayer.setDataSource(url);
mediaPlayer.prepare(); // might take long! (for buffering, etc)
mediaPlayer.start();
```

使用Service:
```java
public class MyService extends Service implements MediaPlayer.OnPreparedListener {
    private static final ACTION_PLAY = "com.example.action.PLAY";
    MediaPlayer mMediaPlayer = null;

    public int onStartCommand(Intent intent, int flags, int startId) {
        ...
        if (intent.getAction().equals(ACTION_PLAY)) {
            mMediaPlayer = ... // initialize it here
            mMediaPlayer.setOnPreparedListener(this);
            mMediaPlayer.prepareAsync(); // prepare async to not block main thread
        }
    }

    /** Called when MediaPlayer is ready */
    public void onPrepared(MediaPlayer player) {
        player.start();
    }
}
```

处理asynchronous errors
```java
public class MyService extends Service implements MediaPlayer.OnErrorListener {
    MediaPlayer mMediaPlayer;

    public void initMediaPlayer() {
        // ...initialize the MediaPlayer here...

        mMediaPlayer.setOnErrorListener(this);
    }

    @Override
    public boolean onError(MediaPlayer mp, int what, int extra) {
        // ... react appropriately ...
        // The MediaPlayer has moved to the Error state, must be reset!
    }
}
```

Using wake locks
To ensure that the CPU continues running while your MediaPlayer is playing, call the setWakeMode() method when initializing your MediaPlayer. Once you do, the MediaPlayer holds the specified lock while playing and releases the lock when paused or stopped:
```java
mMediaPlayer = new MediaPlayer();
// ... other initialization here ...
mMediaPlayer.setWakeMode(getApplicationContext(), PowerManager.PARTIAL_WAKE_LOCK);
```

```java
WifiLock wifiLock = ((WifiManager) getSystemService(Context.WIFI_SERVICE))
    .createWifiLock(WifiManager.WIFI_MODE_FULL, "mylock");

wifiLock.acquire();
```

When you pause or stop your media, or when you no longer need the network, you should release the lock:

```java
wifiLock.release();
```

Running as a foreground service
```java
String songName;
// assign the song name to songName
PendingIntent pi = PendingIntent.getActivity(getApplicationContext(), 0,
                new Intent(getApplicationContext(), MainActivity.class),
                PendingIntent.FLAG_UPDATE_CURRENT);
Notification notification = new Notification();
notification.tickerText = text;
notification.icon = R.drawable.play0;
notification.flags |= Notification.FLAG_ONGOING_EVENT;
notification.setLatestEventInfo(getApplicationContext(), "MusicPlayerSample",
                "Playing: " + songName, pi);
startForeground(NOTIFICATION_ID, notification);
```

播放服务代码例子:
```java
package com.xxxxx.xxxxx.service;

import java.io.IOException;

import android.app.Service;
import android.content.Context;
import android.content.Intent;
import android.media.AudioManager;
import android.media.MediaPlayer;
import android.media.MediaPlayer.OnBufferingUpdateListener;
import android.media.MediaPlayer.OnCompletionListener;
import android.media.MediaPlayer.OnErrorListener;
import android.media.MediaPlayer.OnPreparedListener;
import android.media.MediaPlayer.OnSeekCompleteListener;
import android.net.wifi.WifiManager;
import android.net.wifi.WifiManager.WifiLock;
import android.os.Binder;
import android.os.Handler;
import android.os.IBinder;
import android.os.Parcel;
import android.os.PowerManager;
import android.os.RemoteException;
import android.util.Log;

/**
 * 歌曲播放服务2.0
 * 
 * @author shane
 * 
 */
public class PlayService extends Service implements OnPreparedListener, OnErrorListener,
		OnCompletionListener, OnSeekCompleteListener, OnBufferingUpdateListener {
	public final static String ACTION_PREPARING = "准备播放";
	public final static String ACTION_PROGRESS_INFO = "播放进度信息";
	public final static String ACTION_BUFFERING_PROGRESS_INFO = "缓冲进度信息";
	public final static String ACTION_ALERT_INFO = "播放提示信息";
	public final static String ACTION_ERR_REPORT = "播放错误报告";
	public final static String ACTION_PLAY = "播放";
	public final static String ACTION_STOP = "暂停";
	public final static String ACTION_COMPLETE = "播放完毕";
	public final static String ACTION_SEEKTO_COMPLETE = "跳转完成";
	public final static int PLAY_NEW = 0;
	public final static int PLAY = 1;
	public final static int PAUSE = 2;
	public final static int SEEKTO = 3;
	public final static int EXIT = 4;
	private MediaPlayer mMediaPlayer = null; // 播放器
	private WifiLock wifiLock = null; // wifi锁
	private final IBinder mBinder = new LocalBinder();

	private String lastPlayUrl = null;

	public class LocalBinder extends Binder {
		public PlayService getService() {
			return PlayService.this;
		}

		@Override
		protected boolean onTransact(int code, Parcel data, Parcel reply, int flags)
				throws RemoteException {
			try {
				switch (code) {
				case PLAY_NEW:
					String url = data.readString();
					if (lastPlayUrl != url) {
						lastPlayUrl = url;
						playNew(url);
					} else {
						play();
					}
					break;
				case PLAY:
					// 如果上次播放Url为空,表示未播放过任何歌曲,提示选择播放条目
					if (lastPlayUrl == null) {
						Intent mIntent = new Intent(ACTION_ALERT_INFO);
						mIntent.putExtra("msg", "请选择播放条目");
						sendBroadcast(mIntent); // 发送广播
					} else {
						play();
					}
					break;
				case PAUSE:
					if (mMediaPlayer.isPlaying()) {
						mMediaPlayer.pause();
					}
					sendBroadcast(new Intent(ACTION_STOP));
					break;
				case SEEKTO:
					int progress= data.readInt();
					if (mMediaPlayer != null) {
						int msec = progress * mMediaPlayer.getDuration() / 100;
						Log.i("TAG", String.format("seekTo progress=%d,msec/duration=%d/%d", progress, msec, mMediaPlayer.getDuration()));
						mMediaPlayer.seekTo(msec);
						mMediaPlayer.start();
					}
					break;
				case EXIT:
					if (mMediaPlayer != null) {
						if (mMediaPlayer.isPlaying()) {
							mMediaPlayer.stop();
						}
						mMediaPlayer.release();
						mMediaPlayer = null;
					}
					break;
				default:
					break;
				}
			} catch (Exception e) {
				e.printStackTrace();
				if (mMediaPlayer != null) {
					mMediaPlayer.release();
					mMediaPlayer = null;
				}
				Intent mIntent = new Intent(ACTION_ERR_REPORT);
				mIntent.putExtra("msg", "播放发生错误");
				sendBroadcast(mIntent);
			}
			return true;
		}
	}

	// 初始化播放器
	public MediaPlayer initMediaPlayer() {
		MediaPlayer mMediaPlayer = new MediaPlayer();
		// 设置数据类型
		mMediaPlayer.setAudioStreamType(AudioManager.STREAM_MUSIC);
		// 不重复播放
		mMediaPlayer.setLooping(false);
		mMediaPlayer.setOnPreparedListener(this);
		mMediaPlayer.setOnErrorListener(this);
		mMediaPlayer.setOnCompletionListener(this);
		mMediaPlayer.setWakeMode(getApplicationContext(), PowerManager.PARTIAL_WAKE_LOCK);
		return mMediaPlayer;
	}

	@Override
	public IBinder onBind(Intent intent) {
		// 获得wifi锁
		wifiLock = ((WifiManager) getSystemService(Context.WIFI_SERVICE)).createWifiLock(
				WifiManager.WIFI_MODE_FULL, "mylock");
		wifiLock.acquire();
		new Thread(sendPlayInfoRunnable).start();
		return mBinder;
	}

	@Override
	public int onStartCommand(Intent intent, int flags, int startId) {
		Log.i("TAG", "PlayService onStartCommand");
		return START_STICKY;
	}

	@Override
	public void onDestroy() {
		super.onDestroy();
		if (wifiLock != null && wifiLock.isHeld()) {
			wifiLock.release(); // 释放wifi锁
		}
	}

	@Override
	public void onPrepared(MediaPlayer mp) {
		// 播放准备好
		mMediaPlayer.start();
		sendBroadcast(new Intent(ACTION_PLAY)); // 发送播放广播

		//发送长度
		Intent intent = new Intent();
		intent.putExtra("duration", mMediaPlayer.getDuration());
		sendBroadcast(intent);
	}

	@Override
	public boolean onError(MediaPlayer mp, int what, int extra) {
		Intent mIntent = new Intent(ACTION_ERR_REPORT);
		mIntent.putExtra("msg", "此歌曲暂时不能播放");
		sendBroadcast(mIntent); // 发送广播
		return false;
	}

	@Override
	public void onCompletion(MediaPlayer mp) {
		sendBroadcast(new Intent(ACTION_COMPLETE));
	}

	@Override
	public void onSeekComplete(MediaPlayer arg0) {
		sendBroadcast(new Intent(ACTION_SEEKTO_COMPLETE));
	}

	@Override
	public void onBufferingUpdate(MediaPlayer arg0, int progress) {
		Intent intent = new Intent(ACTION_BUFFERING_PROGRESS_INFO);
		intent.putExtra("progress", progress);
		sendBroadcast(intent);
	}

	Handler timer = new Handler();
	Runnable sendPlayInfoRunnable = new Runnable() {

		@Override
		public void run() {
			if (mMediaPlayer != null && mMediaPlayer.isPlaying()) {
				Intent intent = new Intent(ACTION_PROGRESS_INFO);
				intent.putExtra("progress", mMediaPlayer.getCurrentPosition() * 100 / mMediaPlayer.getDuration());
				sendBroadcast(intent);
			}
			timer.postDelayed(this, 1000);
		}
	};

	private void playNew(String url) throws IllegalArgumentException, SecurityException,
			IllegalStateException, IOException {
		if (mMediaPlayer != null) {
			if (mMediaPlayer.isPlaying()) {
				mMediaPlayer.stop();
			}
			ReleaseMediaPlayer releaseMediaPlayer = new ReleaseMediaPlayer(mMediaPlayer);
			releaseMediaPlayer.start();
		}
		mMediaPlayer = initMediaPlayer();
		mMediaPlayer.setDataSource(url);
		mMediaPlayer.prepareAsync();
		sendBroadcast(new Intent(ACTION_PREPARING));
	}

	// 继续播放当前歌曲
	private void play() throws IllegalArgumentException, SecurityException, IllegalStateException, IOException {
		if (mMediaPlayer == null) {
			playNew(lastPlayUrl);
		} else {
			if (!mMediaPlayer.isPlaying()) {
				mMediaPlayer.start();
				sendBroadcast(new Intent(ACTION_PLAY)); // 发送播放广播
			}
		}
	}

}

class ReleaseMediaPlayer extends Thread {
	private MediaPlayer mMediaPlayer;

	public ReleaseMediaPlayer(MediaPlayer mediaPlayer) {
		this.mMediaPlayer = mediaPlayer;
	}

	@Override
	public void run() {
		super.run();
		Log.i("TAG", "开始释放:" + mMediaPlayer.toString());
		mMediaPlayer.release();
		Log.i("TAG", "结束释放:" + mMediaPlayer.toString());
	}
}

```
