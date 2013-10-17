AFDownloadRequestOperation
==========================

A progressive download operation for AFNetworking. I wrote this to support large PDF downloads in [PSPDFKit, my commercial iOS PDF framework](http://pspdfkit.com), but it works for any file type.

While AFNetworking already supports downloading files, this class has additional support to resume a partial download, uses a temporary directory and has a special block that helps with calculating the correct download progress.

AFDownloadRequestOperation is smart with choosing the correct targetPath. If you set a folder, the file name of the downloaded URL will be used, else the file name that is already set.

AFDownloadRequestOperation also relays any NSError that happened during a file operation to the faulure block.

With partially resumed files, the progress delegate needs additional info. The server might only have a few totalByesExpected, but we wanna show the correct value that includes the previous progress.

``` objective-c
    [pdfRequest setProgressiveDownloadProgressBlock:^(NSInteger bytesRead, long long totalBytesRead, long long totalBytesExpected, long long totalBytesReadForFile, long long totalBytesExpectedToReadForFile) {
        self.downloadProgress = totalBytesReadForFile/(float)totalBytesExpectedToReadForFile;
    }];
```

The temporary folder will be automatically created on first access, but an be changed. It defaults to ```<app directory>tmp/Incomplete/```. The temp directory will be cleaned by the system on a regular bases; so a resume will only succeed if there's not much time between.

### AFNetworking

This is tested against iOS 6+, AFNetworking 2.0 and uses ARC.


### Creator

[Peter Steinberger](http://github.com/steipete)
[@steipete](https://twitter.com/steipete)

## License

AFDownloadRequestOperation is available under the MIT license. See the LICENSE file for more info.
