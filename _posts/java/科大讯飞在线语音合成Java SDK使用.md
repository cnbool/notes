title: 科大讯飞在线语音合成Java SDK使用
id: 1683
categories:
  - Java
date: 2015-04-27 14:37:33
tags:
---

项目中需要将文本转换为语音,使用了科大讯飞在线语音合成SDK,因为离线语音合成是收费的.

**获得appid**

使用讯飞在线语音合成功能需要注册讯飞开放平台账号

讯飞开放平台地址:http://www.xfyun.cn/

注册账户后新建"应用","应用平台"选择"Java"

新建好应用后可以查看到应用的**appid**,例如:5539b565,在代码中需要用到这个值.

**集成Java SDK**

下载SDK后,将开发工具包中 lib 目录下的 Msc.jar 复制到项目中并添加到Build Path(本地Java项目)

将开发包中lib目录下的 libmsc64.so libmsc32.so msc64.dll msc32.dll 放入本地Java工程的根目录

注意:在web项目中使用时,将 libmsc64.so libmsc32.so msc64.dll msc32.dll 放到jre/bin目录才可以调用

**语音合成代码**

首先需要初始化

    SpeechUtility.createUtility(context, SpeechConstant.APPID +"=5539b565");
    

    无声合成

    //1.创建 SpeechSynthesizer 对象
    SpeechSynthesizer mTts= SpeechSynthesizer.createSynthesizer( );
    //2.合成参数设置，详见《iFlytek MSC Reference Manual》 SpeechSynthesizer 类
    mTts.setParameter(SpeechConstant.VOICE_NAME, "xiaoyan");//设置发音人
    mTts.setParameter(SpeechConstant.SPEED, "50");//设置语速，范围 0~100
    mTts.setParameter(SpeechConstant.PITCH, "50");//设置语调，范围 0~100
    mTts.setParameter(SpeechConstant.VOLUME, "50");//设置音量，范围 0~100

    //合成监听器
    SynthesizeToUriListener synthesizeToUriListener = new SynthesizeToUriListener() {
      //progress为合成进度0~100
      public void onBufferProgress(int progress) {}
      //会话合成完成回调接口
      //uri为合成保存地址， error为错误信息，为null时表示合成会话成功
      public void onSynthesizeCompleted(String uri, SpeechError error) {}
    };

    //3.开始合成
    //设置合成音频保存位置（可自定义保存位置），默认保存在“./iflytek.pcm”
    mTts.synthesizeToUri("科大讯飞语音合成测试程序", "D:/test.pcm",synthesizeToUriListener);
    

    * * *

    合成的pcm格式音频采样率16k,采样精度16bit

    需要将合成的pcm添加wav头部,变为wav格式

    网站找到一个工具类,可以给pcm文件加入wav头部,代码如下:

    public class WavWriter {
        /** constant define */
        private static final int SIZE_OF_WAVE_HEADER = 44;
        private static final String CHUNK_ID = "RIFF";
        private static final String FORMAT = "WAVE";
        private static final String SUB_CHUNK1_ID = "fmt ";
        private static final int SUB_CHUNK1_SIZE = 16;
        private static final String SUB_CHUNK2_ID = "data";
        private static final short FORMAT_PCM = 1; // Indicates PCM format.
        private static final short DEFAULT_NUM_CHANNELS = 1;
        private static final short DEFAULT_BITS_PER_SAMPLE = 16;

        /** member properties */
        private RandomAccessFile mInternalWriter;
        private short mNumChannels;
        private int mSampleRate;
        private short mBitsPerSample;

        public WavWriter(File file,int sample) throws IOException {
            init(file, DEFAULT_NUM_CHANNELS, sample,
                    DEFAULT_BITS_PER_SAMPLE);
        }

        private boolean init(File file, short numChannels, int sampleRate,
                short bitsPerSample) throws IOException {
            if (null == file)
                return false;

            mInternalWriter = new RandomAccessFile(file, "rw");
            if (null == mInternalWriter)
                return false;

            mNumChannels = numChannels;
            mSampleRate = sampleRate;
            mBitsPerSample = bitsPerSample;
            byte[] buffer = new byte[SIZE_OF_WAVE_HEADER];
            mInternalWriter.write(buffer);
            return true;
        }

        public void write(byte[] buffer) throws IOException {
            mInternalWriter.write(buffer);
        }

        public void writeChars(String val) throws IOException {
            for (int i = 0; i < val.length(); i++)
                mInternalWriter.write(val.charAt(i));
        }

        public void writeInt(int val) throws IOException {
            mInternalWriter.write(val >> 0);
            mInternalWriter.write(val >> 8);
            mInternalWriter.write(val >> 16);
            mInternalWriter.write(val >> 24);
        }

        public void writeShort(short val) throws IOException {
            mInternalWriter.write(val >> 0);
            mInternalWriter.write(val >> 8);
        }

        public int getDataSize() throws IOException {
            return (int) (mInternalWriter.length() - SIZE_OF_WAVE_HEADER);
        }

        public void writeHeader() throws IOException {
            /* RIFF header */
            mInternalWriter.seek(0);
            writeChars(CHUNK_ID);

            writeInt(36 + getDataSize());
            writeChars(FORMAT);

            /** format chunk */
            writeChars(SUB_CHUNK1_ID);
            writeInt(SUB_CHUNK1_SIZE);
            writeShort(FORMAT_PCM);
            writeShort(mNumChannels);
            writeInt(mSampleRate);

            writeInt(mNumChannels * mSampleRate * mBitsPerSample / 8);
            writeShort((short) (mNumChannels * mBitsPerSample / 8));
            writeShort(mBitsPerSample);

            /** data chunk */
            writeChars(SUB_CHUNK2_ID);
            writeInt(getDataSize());
        }

        public void close() throws IOException {
            if (mInternalWriter != null) {
                mInternalWriter.close();
                mInternalWriter = null;
            }
        }
    }
    

    如果想把合成后的pcm音频转换成wav,合成监听器可以这么写:

    // 合成监听器
    SynthesizeToUriListener synthesizeToUriListener = new SynthesizeToUriListener() {
        // progress为合成进度0~100
        public void onBufferProgress(int progress) {
        }

        // 会话合成完成回调接口
        // uri为合成保存地址， error为错误信息，为null时表示合成会话成功
        public void onSynthesizeCompleted(String uri, SpeechError error) {
            try {
                if (error == null) {
                    File tmpWav = new File(uri);
                    if (tmpWav.exists() && tmpWav.isFile()) {
                        // 生成的PCM文件写入WAV头部
                        WavWriter wavWriter = new WavWriter(tmpWav, 16000);
                        wavWriter.writeHeader();
                        tmpWav.delete();
                    }
                }
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
            }
        }
    };
    
