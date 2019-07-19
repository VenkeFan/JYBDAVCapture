# JYBDAVCapture
OCR扫描身份证及银行卡

使用请看博客：https://blog.csdn.net/tiantianios/article/details/82112660

# 转载自：[OCR:iOS身份证（正反面）识别及银行卡识别](https://blog.csdn.net/tiantianios/article/details/82112660)

## 使用

### 1、在你的项目的Info.plist文件中，添加权限描述（Key   Value）

  Privacy - Camera Usage Description 是否允许访问相机。

  Privacy - Photo Library Usage Description 是否允许访问相册。

### 2、JYBD_IDCardRecognition 文件夹直接拖入工程中。

### 3、运行程序

错误一、 ENABLE_BITCODE 错误

解决方法：在TARGETS和PROJECT 两处，中的 Buid Setting 下找到 Enable Bitcode 将其设置为NO； Xcode8 环境下会检测.a 文件， 所以将 Enable Testability 设置为 NO。

错误二、 

Undefined symbols for architecture arm64:

  "_ZIM_SaveImage", referenced from:
  
      ImgSave(tagIMG, char const*) in libbankcard.a(gjimage.o)
      
  "_ZIM_LoadImage", referenced from:
  
      ImgLoad(tagIMG&, char const*) in libbankcard.a(gjimage.o)
      
  "_ZIM_DoneImage", referenced from:
  
      ImgLoad(tagIMG&, char const*) in libbankcard.a(gjimage.o)
      
ld: symbol(s) not found for architecture arm64

clang: error: linker command failed with exit code 1 (use -v to see invocation)

解决办法：在TARGETS和PROJECT 两处中build settings 搜索 ENABLE_TESTABILITY 改为NO

                    注意！如果你是RN项目，Dead Code Stripping  设置为yes

错误三、 duplicate symbols for architecture arm64（.o文件重复导入了）

解决办法 : 把idcardios.a从 Link Binary With Libraries (3 items)里面删了就好了。

注意：只删除idcardios.a，其他不要删。


### 4、在你的项目中的相应处倒入头文件

#import "JYBDBankCardVC.h"
#import "JYBDIDCardVC.h"

#pragma mark - 身份证扫描

- (IBAction)shoot:(UIButton *)sender {

    __weak __typeof__(self) weakSelf = self;

        JYBDIDCardVC *AVCaptureVC = [[JYBDIDCardVC alloc] init];

    AVCaptureVC.finish = ^(JYBDCardIDInfo *info, UIImage *image)

    {

        IDInfoViewController *infoM = [[IDInfoViewController alloc]init];

        infoM.IDInfo = info;

        infoM.IDImage = image;

        [weakSelf.navigationController pushViewController:infoM animated:YES];

    };

   [self.navigationController pushViewController:AVCaptureVC animated:YES];

}

#pragma mark - 银行卡扫描

- (IBAction)shootAction:(UIButton *)sender {

    __weak __typeof__(self) weakSelf = self;

        JYBDBankCardVC *vc = [[JYBDBankCardVC alloc]init];

    vc.finish = ^(JYBDBankCardInfo *info, UIImage *image) {

       IDInfoViewController *infoM = [[IDInfoViewController alloc]init];

        infoM.cardInfo = info;

        infoM.IDImage = image;

        [weakSelf.navigationController pushViewController:infoM animated:YES];

    };

    [self.navigationController pushViewController:vc animated:YES];

}

4、使用真机，使用真机，使用真机（不支持模拟器）。大功告成。

5、demo 地址：https://github.com/tiantianios/JYBDAVCapture.git

6、扫描属于本地扫描，没有次数限制。

7、有问题留言或联系作者qq:1269456913。

8、可以转载，但要注明出处。
