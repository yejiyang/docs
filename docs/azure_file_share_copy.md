# Copying Files Between Azure File Shares Using AzCopy

This guide explains how to use AzCopy to copy files from one Azure File Share to another.

## Prerequisites

- [AzCopy installed](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10) on your system
- SAS (Shared Access Signature) tokens for both source and destination file shares

## Command Structure

The basic syntax for copying files between Azure File Shares is:

```bash
azcopy copy \
  "<source-file-url-with-sas>" \
  "<destination-share-url-with-sas>" \
  --from-to=FileFile
```

### Parameters Explained

- `source-file-url-with-sas`: The complete URL to your source file, including the SAS token
- `destination-share-url-with-sas`: The complete URL to your destination file share, including the SAS token
- `--from-to=FileFile`: Specifies that both source and destination are Azure File Shares

### Additional Useful Parameters

- `--recursive`: Copy all subdirectories and their contents
- `--overwrite=true|false`: Whether to overwrite existing files (default is true)
- `--log-level=INFO|DEBUG|WARNING|ERROR`: Set logging detail level
- `--cap-mbps`: Cap the transfer rate in megabits per second

## Example

Here's an example of copying a file named `daily-FM_20250528033702.dump` from one file share to another:

```bash
azcopy copy \
  "https://source-account.file.core.windows.net/myshare/myfile.dump?sv=2023-11-03&st=...&se=...&sr=f&sp=r&sig=..." \
  "https://destination-account.file.core.windows.net/myshare?sv=2023-11-03&st=...&se=...&sr=s&sp=rcwl&sig=..." \
  --from-to=FileFile \
  --overwrite=true \
  --log-level=INFO
```

### Example with Multiple Files
```bash
# Copy all .dump files recursively
azcopy copy \
  "https://source-account.file.core.windows.net/myshare/*?sv=..." \
  "https://destination-account.file.core.windows.net/myshare?sv=..." \
  --from-to=FileFile \
  --recursive \
  --include-pattern="*20250527*.dump"
```

Note: This command will:
- Scan all files in the source share recursively (`--recursive`)
- Only copy files that match the pattern `*20250527*.dump` (`--include-pattern`)
- The wildcard (*) in the source URL path must be at the end of the path section

## Important Notes

1. **SAS Tokens**:
   - Ensure the source SAS token has at least Read (r) permissions
   - Ensure the destination SAS token has Read, Create, Write, and List (rcwl) permissions
   - SAS tokens should be valid for the duration of the copy operation

2. **URLs**:
   - Source URL should point to the specific file you want to copy
   - Destination URL should point to the file share where you want to copy the file

3. **Security**:
   - Never share or commit SAS tokens to version control
   - Use appropriate token expiration times
   - Use minimum required permissions for SAS tokens

## Getting URLs with SAS Tokens Using Azure Storage Explorer

The easiest way to obtain URLs with SAS tokens is using Microsoft Azure Storage Explorer:

1. **Install Azure Storage Explorer**:
   - Download and install [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/)
   - Sign in with your Azure account

2. **Generate SAS URL for Source File**:
   - Navigate to your source storage account
   - Expand "File Shares"
   - Find your source file
   - Right-click on the file
   - Select "Get Shared Access Signature"
   - Configure the settings:
     - Set appropriate start and expiry times
     - Select required permissions (Read for source)
     - Click "Create"
   - Copy the "URL" field - this is your complete source URL with SAS token

3. **Generate SAS URL for Destination Share**:
   - Navigate to your destination storage account
   - Expand "File Shares"
   - Right-click on the destination share (folder)
   - Select "Get Shared Access Signature"
   - Configure the settings:
     - Set appropriate start and expiry times
     - Select required permissions (Read, Create, Write, List)
     - Click "Create"
   - Copy the "URL" field - this is your complete destination URL with SAS token

4. **Tips**:
   - Make sure to set an appropriate expiry time that gives you enough time to complete the copy operation
   - For large files, consider setting longer expiry times
   - The URLs will look similar to the examples shown in the Command Structure section above

## Common Issues

- If the copy fails, verify that:
  - Both SAS tokens are valid and haven't expired
  - The source file exists
  - You have sufficient permissions on both source and destination
  - The destination file share has enough space

## Additional Resources

- [AzCopy Documentation](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10)
- [Azure File Shares Documentation](https://learn.microsoft.com/en-us/azure/storage/files/storage-files-introduction)
- [SAS Token Documentation](https://learn.microsoft.com/en-us/azure/storage/common/storage-sas-overview)
- [Azure Storage Performance Guidelines](https://learn.microsoft.com/en-us/azure/storage/common/storage-performance-checklist)
- [Azure Storage Security Guide](https://learn.microsoft.com/en-us/azure/storage/common/storage-security-guide) 