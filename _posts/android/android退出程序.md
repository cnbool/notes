title: Android退出程序
categories:
  - Android
date: 2016-03-08 14:00:08
tags:
---

文章源自:http://blog.csdn.net/soul_code/article/details/50453934

## 设置MainActivity的加载模式为singleTask ##
```
android:launchMode="singleTask"
```

## 方法一:在MainActivity中双击返回键退出 ## 
```java
		private boolean mIsExit;
		
    @Override
    /**
     * 双击返回键退出
     */
    public boolean onKeyDown(int keyCode, KeyEvent event) {

        if (keyCode == KeyEvent.KEYCODE_BACK) {
            if (mIsExit) {
                this.finish();
            } else {
                Toast.makeText(this, "再按一次退出", Toast.LENGTH_SHORT).show();
                mIsExit = true;
                new Handler().postDelayed(new Runnable() {
                    @Override
                    public void run() {
                        mIsExit = false;
                    }
                }, 2000);
            }
            return true;
        }

        return super.onKeyDown(keyCode, event);
    }
```

## 方法二:在Intent中添加退出的tag##
```java
		private static final String TAG_EXIT = "EXIT";

    @Override
    protected void onNewIntent(Intent intent) {
        super.onNewIntent(intent);
        if (intent != null) {
            boolean isExit = intent.getBooleanExtra(TAG_EXIT, false);
            if (isExit) {
                this.finish();
            }
        }
    }
```

退出
```
		Intent intent = new Intent(this, MainActivity.class);
    intent.putExtra(MainActivity.TAG_EXIT, true);
    startActivity(intent);
```