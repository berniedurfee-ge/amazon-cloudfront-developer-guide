# Using Amazon S3 Origins and Custom Origins for Web Distributions<a name="DownloadDistS3AndCustomOrigins"></a>

When you create a web distribution, you specify where CloudFront sends requests for the files that it distributes to edge locations\. CloudFront supports using Amazon S3 buckets and HTTP servers \(for example, web servers\) as origins\.

## Using Amazon S3 Buckets for Your Origin<a name="concept_S3Origin"></a>

When you use Amazon S3 as an origin for your distribution, you place any objects that you want CloudFront to deliver in an Amazon S3 bucket\. You can use any method that is supported by Amazon S3 to get your objects into Amazon S3, for example, the Amazon S3 console or API, or a third\-party tool\. You can create a hierarchy in your bucket to store the objects, just as you would with any other Amazon S3 bucket\.

Using an existing Amazon S3 bucket as your CloudFront origin server doesn't change the bucket in any way; you can still use it as you normally would to store and access Amazon S3 objects at the standard Amazon S3 price\. You incur regular Amazon S3 charges for storing the objects in the bucket\. For more information about the charges to use CloudFront, see [CloudFront Reports](reports.md)\.

**Important**  
For your bucket to work with CloudFront, the name must conform to DNS naming requirements\. For more information, go to [Bucket Restrictions and Limitations](http://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) in the *Amazon Simple Storage Service Developer Guide*\.

When you specify the Amazon S3 bucket that you want CloudFront to get objects from, how you specify the bucket name depends on whether you have configured the bucket as a website endpoint:

**The bucket is not configured as a website endpoint**  
In general, use the following format:  
`bucket-name.s3.amazonaws.com`  
If your bucket is in the US Standard region and you want Amazon S3 to route requests to a facility in Northern Virginia, use the following format:  
`bucket-name.s3-external-1.amazonaws.com`   
When you specify the bucket name in this format, you can use the following CloudFront features:  
+ Configure CloudFront to communicate with your Amazon S3 bucket using SSL\. For more information, see [Using HTTPS with CloudFront](using-https.md)\.
+ Use an origin access identity to require that your users access your content using CloudFront URLs, not by using Amazon S3 URLs\. For more information, see [Using an Origin Access Identity to Restrict Access to Your Amazon S3 Content](private-content-restricting-access-to-s3.md)\.
+ Update the content of your bucket by submitting `POST` and `PUT` requests to CloudFront\. For more information, see [HTTP Methods](RequestAndResponseBehaviorS3Origin.md#RequestS3HTTPMethods) in the topic [How CloudFront Processes and Forwards Requests to Your Amazon S3 Origin Server](RequestAndResponseBehaviorS3Origin.md#RequestBehaviorS3Origin)\.

**The bucket is configured as a website endpoint**  
Enter the Amazon S3 static website hosting endpoint for your bucket\. This value appears in the Amazon S3 console, on the **Properties** page under **Static Website Hosting**\.  
When you specify the bucket name in this format, you can use Amazon S3 redirects and Amazon S3 custom error documents\. \(CloudFront also provides custom error pages\. For more information, see [Customizing Error Responses](custom-error-pages.md)\.\) For more information about Amazon S3 features, see the [Amazon S3 documentation](http://aws.amazon.com/documentation/s3/)\.

Do not specify the bucket using the following formats:
+ The Amazon S3 path style, `s3.amazonaws.com/bucket-name`
+ The Amazon S3 CNAME, if any

## Using Amazon EC2 or Other Custom Origins<a name="concept_CustomOrigin"></a>

A custom origin is an HTTP server, for example, a web server\. The HTTP server can be an Amazon EC2 instance or an HTTP server that you manage privately\. When you use a custom origin, you specify the DNS name of the server, along with the HTTP and HTTPS ports and the protocol that you want CloudFront to use when fetching objects from your origin\. 

Most CloudFront features are supported when you use a custom origin with the following exceptions:
+ **RTMP distributions**—Not supported\.
+ **Private content**—Although you can use a signed URL to distribute content from a custom origin, for CloudFront to access the custom origin, the origin must remain publicly accessible\. For more information, see [Serving Private Content through CloudFront](PrivateContent.md)\.

For information about requirements and recommendations when using custom origins, see [Requirements and Recommendations for Using Amazon EC2 and Other Custom Origins](CustomOriginBestPractices.md)\.