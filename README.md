#   MNZXingCode 快速集成zxing二维码扫描

##  快速集成zxing二维码扫描，生成二维码，可配置相册，闪光灯，相机可以调整焦距放大缩小，自定义扫描线颜色，自定义背景颜色，自定义遮罩层，自定义Activity，多个二维码扫码（zxing:core:3.3.3/3.4.0）
[![](https://jitpack.io/v/maning0303/MNZXingCode.svg)](https://jitpack.io/#maning0303/MNZXingCode)


### （推荐）MNMLKitScanner 基于MLKit集成识别，速度比zxing更快，可以同时支持多个二维码和多个条形码扫描
[MNMLKitScanner](https://github.com/maning0303/MNMLKitScanner)

##  功能：
    1：生成二维码，支持修改颜色和边距，支持带Logo
    2：二维码扫描，手势缩放，无拉伸，样式自定义
    3：相册中选取图片识别
    4：闪光灯开关
    5: 相机可以调整焦距放大缩小
    6: 完全自定义遮罩层
    7: 自定义Activity
    8: 支持微信，支付宝类似多个二维码扫码


## 截图:

<div align="center">
<img src = "screenshots/mn_zxing_screenshot_000.jpg" width=180 >
<img src = "screenshots/mn_zxing_screenshot_001.jpeg" width=180 >
<img src = "screenshots/mn_zxing_screenshot_002.png" width=180 >
<img src = "screenshots/mn_zxing_screenshot_003.jpeg" width=180 >
<img src = "screenshots/mn_zxing_screenshot_004.jpg" width=180 >
<img src = "screenshots/mn_zxing_screenshot_005.jpg" width=180 >
<img src = "screenshots/mn_zxing_screenshot_006.jpg" width=180 >
</div>

## 如何添加
### Gradle添加：
#### 1.在Project的build.gradle中添加仓库地址

``` gradle
	allprojects {
		repositories {
			...
			maven { url "https://jitpack.io" }
		}
	}
```

#### 2.在Module目录下的build.gradle中添加依赖
``` gradle
	dependencies {

            //zxing 3.4.0 不兼容7.0以下版本，必须java8+
            implementation 'com.google.zxing:core:3.4.0'
            implementation 'com.github.maning0303:MNZXingCode:V2.1.9'

            //需要支持7.0以下
            implementation 'com.google.zxing:core:3.3.3'
            implementation 'com.github.maning0303:MNZXingCode:V2.1.9'

	}
```

## 使用方法:
###  进入需要提前申请相机权限；进入需要提前申请相机权限；进入需要提前申请相机权限；


``` java
        1：开始扫描：
            //默认扫描
            MNScanManager.startScan(this, new MNScanCallback() {
                   @Override
                   public void onActivityResult(int resultCode, Intent data) {
                    switch (resultCode) {
                        case MNScanManager.RESULT_SUCCESS:
                            String resultSuccess = data.getStringExtra(MNScanManager.INTENT_KEY_RESULT_SUCCESS);
                            break;
                        case MNScanManager.RESULT_FAIL:
                            String resultError = data.getStringExtra(MNScanManager.INTENT_KEY_RESULT_ERROR);
                            break;
                        case MNScanManager.RESULT_CANCLE:
                            showToast("取消扫码");
                            break;
                    }
                   }
            });
            
            
            //自定义扫描
            MNScanConfig scanConfig = new MNScanConfig.Builder()
                            //设置完成震动
                            .isShowVibrate(false)
                            //扫描完成声音
                            .isShowBeep(true)
                            //显示相册功能
                            .isShowPhotoAlbum(true)
                            //显示闪光灯
                            .isShowLightController(true)
                            //打开扫描页面的动画
                            .setActivityOpenAnime(R.anim.activity_anmie_in)
                            //退出扫描页面动画
                            .setActivityExitAnime(R.anim.activity_anmie_out)
                            //自定义文案
                            .setScanHintText("请将二维码放入框中")
                            //自定义文案颜色
                            .setScanHintTextColor("#FFFF00")
                            //自定义文案大小（单位sp）
                            .setScanHintTextSize(16)
                            //扫描线的颜色
                            .setScanColor("#FFFF00")
                            //是否显示缩放控制器
                            .isShowZoomController(true)
                            //显示缩放控制器位置
                            .setZoomControllerLocation(MNScanConfig.ZoomControllerLocation.Bottom)
                            //扫描线样式
                            .setLaserStyle(MNScanConfig.LaserStyle.Grid)
                            //背景颜色
                            .setBgColor("#33FF0000")
                            //网格扫描线的列数
                            .setGridScanLineColumn(30)
                            //网格高度
                            .setGridScanLineHeight(150)
                            //高度偏移值（单位px）+向上偏移，-向下偏移
                            .setScanFrameHeightOffsets(150)
                            //是否全屏范围扫描
                            .setFullScreenScan(true)
                            //二维码标记点
                            .isShowResultPoint(mCbScanPoint.isChecked())
                            .setResultPointConfigs(36, 12, 2, "#FFFFFFFF", "#CC00A81F")
                            //状态栏设置：颜色，是否黑色字体
                            .setStatusBarConfigs("#00000000", true)
                            //是否支持多二维码同时扫出,默认false,多二维码状态不支持条形码
                            .setSupportMultiQRCode(true)
                            //自定义遮罩
                            .setCustomShadeViewLayoutID(R.layout.layout_custom_view, new MNCustomViewBindCallback() {
                                @Override
                                public void onBindView(View customView) {
                                    //TODO:通过findviewById 获取View
                                }
                            })
                            .builder();
            MNScanManager.startScan(this, scanConfig, new MNScanCallback() {
                @Override
                public void onActivityResult(int resultCode, Intent data) {
                    switch (resultCode) {
                        case MNScanManager.RESULT_SUCCESS:
                            String resultSuccess = data.getStringExtra(MNScanManager.INTENT_KEY_RESULT_SUCCESS);
                            break;
                        case MNScanManager.RESULT_FAIL:
                            String resultError = data.getStringExtra(MNScanManager.INTENT_KEY_RESULT_ERROR);
                            break;
                        case MNScanManager.RESULT_CANCLE:
                            showToast("取消扫码");
                            break;
                    }
                }
            });


            //自定义Activity
            //1.xml添加：
            <com.google.zxing.client.android.view.ScanSurfaceView
                    android:id="@+id/scan_surface_view"
                    android:layout_width="match_parent"
                    android:layout_height="match_parent" />

            //2.Activity实例代码：
            public class ScanActivity extends AppCompatActivity {

                private ScanSurfaceView mScanSurfaceView;

                @Override
                protected void onCreate(Bundle savedInstanceState) {
                    super.onCreate(savedInstanceState);
                    setContentView(R.layout.activity_scan);
                    initView();
                }

                private void initView() {
                    mScanSurfaceView = (ScanSurfaceView) findViewById(R.id.scan_surface_view);
                    mScanSurfaceView.init(this);
                    MNScanConfig scanConfig = new MNScanConfig.Builder()
                            .setSupportZoom(false)
                            .builder();
                    mScanSurfaceView.setScanConfig(scanConfig);
                    mScanSurfaceView.setOnScanCallback(new OnScanCallback() {
                        @Override
                        public void onScanSuccess(String resultTxt, Bitmap barcode) {
                            Toast.makeText(ScanActivity.this, "成功：" + resultTxt, Toast.LENGTH_SHORT).show();
                            //关闭页面
                            finish();
                        }

                        @Override
                        public void onFail(String msg) {
                            Toast.makeText(ScanActivity.this, "失败：" + msg, Toast.LENGTH_SHORT).show();
                            //关闭页面
                            finish();
                        }
                    });
                }

                @Override
                protected void onResume() {
                    super.onResume();
                    mScanSurfaceView.onResume();
                }

                @Override
                protected void onPause() {
                    super.onPause();
                    mScanSurfaceView.onPause();
                }

                @Override
                protected void onDestroy() {
                    super.onDestroy();
                    mScanSurfaceView.onDestroy();
                }

                @Override
                public void onBackPressed() {
                    //多点扫描结果点显示，可以取消
                    if (mScanSurfaceView != null && mScanSurfaceView.isResultPointViewShow()) {
                        mScanSurfaceView.hideResultPointView();
                        mScanSurfaceView.restartScan();
                        return;
                    }
                    super.onBackPressed();
                }
            }

        3：生成二维码：
        	Bitmap qrImage = ZXingUtils.createQRCodeImage("xxxxxx");
            /**
             * 生成带logo的二维码，logo默认为二维码的1/5
             *
             * @param text                   需要生成二维码的内容
             * @param size                   需要生成二维码的大小
             * @param margin                 二维码边距
             * @param black_color            二维码颜色
             * @param white_color            二维码背景颜色
             * @param error_correction_level 容错率 L：7% M：15% Q：25% H：35%
             * @param logo_bitmap            logo文件
             * @return bitmap
             */
            public static Bitmap createQRCodeImage(String text, int size, int margin, int black_color, int white_color, String error_correction_level, Bitmap logo_bitmap) ;
        	
        4：解析图片中的二维码：
        	String code = ZXingUtils.syncDecodeQRCode(String picturePath);
        	String code = ZXingUtils.syncDecodeQRCode(Bitmap bitmap);

        5.提供扫描界面相关方法（自定义遮罩层会使用）：
            /**
             * 关闭当前页面
             */
            MNScanManager.closeScanPage();

            /**
             * 打开相册扫描图片
             */
            MNScanManager.openAlbumPage();

            /**
             * 打开手电筒
             */
            MNScanManager.openScanLight();

            /**
             * 关闭手电筒
             */
            MNScanManager.closeScanLight();

            /**
             * 手电筒是否开启
             */
            MNScanManager.isLightOn();
```

## 版本记录：
    v2.1.9:
        1.扫描结果点显示位置偏移优化
        2.扫描结果点View支持手动取消
        3.扫描网格线显示优化
        
    v2.1.8:
        1.扫描结果点显示位置偏移修复
        
    v2.1.7:
        1.支持多二维码同时扫出
        2.优化扫描结果点
        3.zxing版本自定义


## 感谢：

[zxing](https://github.com/zxing/zxing)

[BGAQRCode-Android](https://github.com/bingoogolapple/BGAQRCode-Android)

[StatusBarUtil](https://github.com/Ye-Miao/StatusBarUtil)

感谢所有开源的人；



## 推荐:
Name | Describe |
--- | --- |
[GankMM](https://github.com/maning0303/GankMM) | （Material Design & MVP & Retrofit + OKHttp & RecyclerView ...）Gank.io Android客户端：每天一张美女图片，一个视频短片，若干Android，iOS等程序干货，周一到周五每天更新，数据全部由 干货集中营 提供,持续更新。 |
[MNUpdateAPK](https://github.com/maning0303/MNUpdateAPK) | Android APK 版本更新的下载和安装,适配7.0,简单方便。 |
[MNImageBrowser](https://github.com/maning0303/MNImageBrowser) | 交互特效的图片浏览框架,微信向下滑动动态关闭 |
[MNZXingCode](https://github.com/maning0303/MNZXingCode) | 快速集成二维码扫描和生成二维码 |
[MNMLKitScanner](https://github.com/maning0303/MNMLKitScanner) | 基于Google MLKit 快速集成二维码扫描,速度比zxing快 |
[MNPasswordEditText](https://github.com/maning0303/MNPasswordEditText) | 类似微信支付宝的密码输入框。 |
[MClearEditText](https://github.com/maning0303/MClearEditText) | 带有删除功能的EditText |
[MNCrashMonitor](https://github.com/maning0303/MNCrashMonitor) | Debug监听程序崩溃日志,展示崩溃日志列表，方便自己平时调试。 |
[MNProgressHUD](https://github.com/maning0303/MNProgressHUD) | MNProgressHUD是对常用的自定义弹框封装,加载ProgressDialog,状态显示的StatusDialog和自定义Toast,支持背景颜色,圆角,边框和文字的自定义。 |
[SwitcherView](https://github.com/maning0303/SwitcherView) | 垂直滚动的广告栏文字展示。 |
[MNVideoPlayer](https://github.com/maning0303/MNVideoPlayer) | SurfaceView + MediaPlayer 实现的视频播放器，支持横竖屏切换，手势快进快退、调节音量，亮度等。------代码简单，新手可以看一看。 |
[MNChangeSkin](https://github.com/maning0303/MNChangeSkin) | Android夜间模式，通过Theme实现 |
[MNXUtilsDB](https://github.com/maning0303/MNXUtilsDB) | xUtils3 数据库模块单独抽取出来，方便使用。 |
[MNCalendar](https://github.com/maning0303/MNCalendar) | 简单的日历控件练习，水平方向日历支持手势滑动切换，跳转月份；垂直方向日历选取区间范围。 |
[MNSwipeToLoadDemo](https://github.com/maning0303/MNSwipeToLoadDemo) | 利用SwipeToLoadLayout实现的各种下拉刷新效果（饿了吗，京东，百度外卖，美团外卖，天猫下拉刷新等）。 |

