# 入门用户的aws s3运维操作与WEB开发指南



## （1） S3基本知识

1.	S3基础和网页版使用 控制台： https://s3.console.aws.amazon.com/

2.	入门教程（就是普通的上传下载文件，类似云盘操作）https://docs.aws.amazon.com/AmazonS3/latest/gsg/SigningUpforS3.html

3.	进阶教程：如何将文件和文件夹上传至 S3 存储桶？https://docs.aws.amazon.com/zh_cn/AmazonS3/latest/user-guide/upload-objects.html

4.	Amazon S3 + Amazon CloudFront: 云中的绝妙搭配，存储和加速内容，同时提高安全性并降低成本：
https://aws.amazon.com/cn/blogs/china/amazon-s3-amazon-cloudfront-%E4%BA%91%E4%B8%AD%E7%9A%84%E7%BB%9D%E5%A6%99%E6%90%AD%E9%85%8D/



## （2-1）S3上传操作（开发角度）

1.	开发首先看：S3最简单的上传：在单个操作中上传对象（小于5GB的单个文件上传）：https://docs.aws.amazon.com/zh_cn/AmazonS3/latest/dev/UploadInSingleOp.html
**下载文档的示例代码，在本地修改下就能使用。**

![image-20230914143216354](https://raw.githubusercontent.com/liangyimingcom/storage/master/PicGo/image-20230914143216354.png)




2. 请运维人员提供AK/SK给到研发同事，以便测试。以JAVA为例，环境配置方法如下：https://docs.aws.amazon.com/zh_cn/sdk-for-java/v2/developer-guide/setup-credentials.html

3. Amazon S3 更多操作：允许您存储、检索和删除对象。您可以检索整个对象或部分对象：https://docs.aws.amazon.com/zh_cn/AmazonS3/latest/dev/ObjectOperations.html

4. 示例：通过浏览器将照片上传到 Amazon S3
   https://docs.aws.amazon.com/zh_cn/sdk-for-javascript/v2/developer-guide/s3-example-photo-album.html

   

5. 大文件上传
   multipart 的基本概念和限制：
   https://docs.aws.amazon.com/AmazonS3/latest/dev/uploadobjusingmpu.html
   https://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html （REST API）
   https://docs.aws.amazon.com/AmazonS3/latest/dev/qfacts.html

multipart(REST API): https://docs.aws.amazon.com/AmazonS3/latest/API/mpUploadInitiate.html




6. 前端Javascript直接上传S3（）：https://aws.amazon.com/cn/blogs/china/s3-multipul-upload-practice/

   

7. 掌握了以上理论后，可以去GitHub搜索多种编程语言的模板代码，网上也有很多中英文教程可以借鉴。
   •	从浏览器将照片上传到 Amazon S3 https://docs.aws.amazon.com/zh_cn/sdk-for-javascript/v2/developer-guide/s3-example-photo-album.html
   •	https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascript/example_code/s3
   •	https://github.com/awsdocs/aws-doc-sdk-examples

   

8. 进阶教程，简单而且有意思的缩略图自动生成教程：基于S3的图片处理服务，客户端根据基于HTTP URL API 请求实时生成不同分辨率的图片。https://aws.amazon.com/cn/blogs/china/image-processing-service-based-on-s3/

***以下是完全不推荐的方式。虽然可实现简单网页直接上传到S3，但是会造成AK/SK秘钥泄露带来安全风险（放在这里是作为反面教材）：***
*•	https://blog.csdn.net/sdiudui/article/details/79928631 将照片通过浏览器上传到Amazon S3服务*
*•	https://blog.csdn.net/qq1147093833/article/details/80267542 aws s3直接通过JavaScript上传文件*
*•	https://www.programminghunter.com/article/9782905557/ 上传文件到 aws s3存储桶*



补充：

9. vue 前端项目上传文件到AWS Amazon S3 存储桶（此方法直接通过 Upload 方法上传文件，存在安全隐患，AKSK有暴露风险。AKSK请务必只给S3的权限）：https://blog.chensihang.cn/Vue%20%E4%B8%8A%E4%BC%A0%E6%96%87%E4%BB%B6%E5%88%B0AWS%20S3/

10. vue + axios 通过 S3 预签名地址上传文件（相比上面的方案消除了AKSK暴露风险，普遍做法是后端提供文件上传预签名地址，前端根据预签名地址进行文件上传操作）：
https://blog.csdn.net/createNo_1/article/details/114268110



## （2-2）S3上传操作（ios/Android）

官方入口： https://docs.aws.amazon.com/AmazonS3/latest/dev/using-mobile-sdks.html

![image-20230914143351766](https://raw.githubusercontent.com/liangyimingcom/storage/master/PicGo/image-20230914143351766.png)



还有一个直接抄代码的捷径，就是github。

https://github.com/search?q=S3+upload+ios&type=Repositories
这是IOS的，挑星星多的。

![image-20230914143402891](https://raw.githubusercontent.com/liangyimingcom/storage/master/PicGo/image-20230914143402891.png)



Android的
https://github.com/search?q=S3+upload+android&type=Repositories

![image-20230914143412443](https://raw.githubusercontent.com/liangyimingcom/storage/master/PicGo/image-20230914143412443.png)





## （3）S3上传操作（运维角度）

•	通过 AWS CLI 使用高级别 (s3) 命令 – aws全球区：https://docs.aws.amazon.com/zh_cn/cli/latest/userguide/cli-services-s3-commands.html

•	通过 AWS CLI 使用高级别 (s3) 命令 – aws中国区：https://docs.amazonaws.cn/cli/latest/userguide/cli-services-s3-commands.html

S3 命令行工具的使用
aws s3api list-multipart-uploads --bucket [bucket_name]


aws s3api abort-multipart-upload --bucket [bucket_name] --key [key] --upload-id [uploadid]

AWS_PROFILE=develop aws s3 ls s3://[bucket_name]

AWS_PROFILE=develop aws s3api list-multipart-uploads --bucket [bucket_name]

AWS_PROFILE=develop aws s3api list-parts --bucket [bucket_name] --key [key] --upload-id [upload_id]

•	如何使用AWS 命令行分段上传超过5GB的大文件。亚马逊S3提供了在单个操作中上传文件和分段上传文件两种方式。使用单个操作上传，每次可以上传最大5GB的文件。如果使用分段上传来上传文件，可以 上传最大大小为5TB的文件。：https://aws.amazon.com/cn/blogs/china/uploading-using-s3/



## （4）S3直接作为本地盘操作（运维角度）

•	可以利用S3fs在Amazon EC2 Linux实例上挂载S3存储桶，S3fs是基于FUSE的文件系统，允许Linux和Mac Os X 挂载S3的存储桶在本地文件系统，S3fs能够保持对象原来的格式。教程如下：https://aws.amazon.com/cn/blogs/china/s3fs-amazon-ec2-linux/

•	aws云上快速搭建 NFS，File Gateway文件网关提供了一个文件接口，让您可以使用行业标准 NFS 文件协议将文件作为对象存储在 Amazon S3 中，并通过 NFS 从您的数据中心或 Amazon EC2 访问这些文件：https://aws.amazon.com/cn/blogs/china/how-to-establish-nfs/



## 引用：

1） AWS S3 上传下载工具 - 配置与使用指南(**适用于MAC版本的S3 cyberduck配置指南**]) https://github.com/liangyimingcom/AWS-S3-upload-and-download-tool-configuration-and-usage-guide_available-for-AWS-China/blob/readme-edits/README.md

2）世界上最简单的S3命令行说明(文件备份与恢复) The simplest S3 command line guide in the world for Files Backup/Restore https://github.com/liangyimingcom/The-simplest-S3-command-line-guide-in-the-world-for-Files-Backup-Restore
