---
title: Using Powershell to Restore Files from the Windows Recycling bin
author: carl_zhou
date: 2023-09-15 10:33:00 +0800
categories: [Programming]
tags: [windows, powershell, onedrive]
pin: false
math: true
mermaid: true
image:
  path: /assets/powershell.png
  lqip: data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAKCAYAAACNMs+9AAAAAklEQVR4AewaftIAAAEbSURBVJXBMUtCURiA4feee7vdEiWzRYIQgmhya9CGllqDWoX+QlMIDi1Fv6ItqD2aC4KMaqhFCKOgoQYDMbLAc8/5vu5PqOfhrwIya63O9GRxblvFG/UpIg71KeJTVByZu4jMRKG8HkbJbqOunN190RuAqqDi8G5EEJgDQyYIwmXxltvuN63NHPk4RZxFnEW8JTBh25BRpC7O0nkdcXQ+YK8xQ8gI8Rb1TpJC+dqsNh+mUV0UbynmhI1agftuH2st4i2Zx9PmVD+Kc6Wa+NQszYdsreQ5uXjn41NZmB2n8zzEjCVXZAwEdfEplZJj5/CF156jWompVhJUBRPGbTKRir9BZf/48gf1hsFwxNPbEBVPFOdkolA+5T9+AQJqjoX2g3juAAAAAElFTkSuQmCC
  alt: Windows Powershell Command Console
---

# The Problem with OneDrive and Cryptomator

I had a drive encrypted by Cryptomator, which I synchronized with my OneDrive account. Because of the way Cryptomator names the encrypted files, OneDrive thinks the files are ransomware related files. Thus the files were deleted from my OneDrive storage, but fortunately they were only moved to the recycling bin on my local computer.
I am not sure why OneDrive developers thought it was a good idea to remove ransomware encrypted files from OneDrive storage, decreasing chances of recovery.

After some research, I discovered I am not the only one with this problem, and this remains an open issue:

<https://github.com/cryptomator/cryptomator/issues/1059>

I ended up with thousands of files in my recycling bin on my local computer, which I had a difficult time restoring using the GUI "restore files". This would often freeze my computer. It was obvious that the GUI was not designed to handle such a scenario.

I had to write a Powershell script to solve the problem.

# Overview of the script

The script accesses "ssfBITBUCKET" which is the user's recycling bin and the various properties of the file. These properties include name, deletion time, parent path(original location of the file).

Using a for loop going through all items in the recycling bin's item list, the deletion date of the file is checked against the $month, $date, and $year specified in the script. 

If the file was deleted before or on this day, then the file is moved from the recycling bin into the parent path, which is the original location of the file.

## The final script

```powershell
New-Variable -Name 'ssfBITBUCKET'                       -Option Constant -Value 0x0A;
New-Variable -Name 'BitBucketDetails_Name'              -Option Constant -Value 0;
New-Variable -Name 'BitBucketDetails_ParentPath'        -Option Constant -Value 1;
New-Variable -Name 'BitBucketDetails_DeletionTimeText'  -Option Constant -Value 2;
New-Variable -Name 'BitBucketDetails_SizeText'          -Option Constant -Value 3;
New-Variable -Name 'BitBucketDetails_Type'              -Option Constant -Value 4;
New-Variable -Name 'BitBucketDetails_LastWriteTimeText' -Option Constant -Value 5;
New-Variable -Name 'BitBucketDetails_CreationTimeText'  -Option Constant -Value 6;
New-Variable -Name 'BitBucketDetails_LastAccessTimeText'-Option Constant -Value 7;
New-Variable -Name 'BitBucketDetails_AttributesText'    -Option Constant -Value 8;

$application = New-Object -ComObject 'Shell.Application';
$bitBucket = $application.NameSpace($ssfBITBUCKET);

#date to restore up to
$month = '10';
$date = '2';
$year = '2021';

foreach ($deletedItem in $bitBucket.Items())
{
    $file = New-Object -TypeName 'PSObject' -Property @{
        # Same as $deletedItem.Name
        Name =               $bitBucket.GetDetailsOf($deletedItem, $BitBucketDetails_Name);
        ParentPath =         $bitBucket.GetDetailsOf($deletedItem, $BitBucketDetails_ParentPath);
        DeletionTimeText =   $bitBucket.GetDetailsOf($deletedItem, $BitBucketDetails_DeletionTimeText);
        Size =               $deletedItem.Size;
        SizeText =           $bitBucket.GetDetailsOf($deletedItem, $BitBucketDetails_SizeText);
        # Same as $deletedItem.Type
        Type =               $bitBucket.GetDetailsOf($deletedItem, $BitBucketDetails_Type);
        LastWriteTime =      $deletedItem.ModifyDate;
        LastWriteTimeText =  $bitBucket.GetDetailsOf($deletedItem, $BitBucketDetails_LastWriteTimeText);
        CreationTimeText =   $bitBucket.GetDetailsOf($deletedItem, $BitBucketDetails_CreationTimeText);
        LastAccessTimeText = $bitBucket.GetDetailsOf($deletedItem, $BitBucketDetails_LastAccessTimeText);
        AttributesText =     $bitBucket.GetDetailsOf($deletedItem, $BitBucketDetails_AttributesText);
        IsFolder =           $deletedItem.IsFolder();
        BitBucketPath =      $deletedItem.Path;
    };
    
    if ($file.DeletionTimeText.Contains($month) -and $file.DeletionTimeText.Contains($date) -and $file.DeletionTimeText.Contains($year)) #full date string matching breaks due to unicode in the timestamp
    {
        $destinationFullPath=-join($file.ParentPath,'\',$file.Name)
        $realFullPath=-join($file.ParentPath,'\',$file.Name)
        Write-Output "Processing: $realFullPath"
        if(!(test-path $file.ParentPath))
        {
            New-Item -ItemType Directory -Force -Path $file.ParentPath
        }
        Move-Item -Path $file.BitBucketPath -Destination $destinationFullPath
    }
}
```

https://gist.github.com/carlzoo/40b42ef26b9bdf2238ce52e6a0835dd2

# Conclusion

I was able to recover thousands of deleted files using this script, where as the GUI would freeze.

I decided to stop using Onedrive due to the fact that it deletes files arbitrarily without the user's consent. I recommend everyone stop using it as well until this behavior is changed.