Quick steps
- Install NGC
```
https://confluence.nvidia.com/display/COS/NGC+CLI+Installation+and+Configuration
```
- Login to `nvcr.io`
```
Enter the following command in terminal

`docker login nvcr.io`

If you have lost or not created your api key you can regenerate it [https://ngc.nvidia.com/setup/api-key](https://ngc.nvidia.com/setup/api-key)

It will ask for credentials then use `$oauthtoken`  as username and the api key as the password
```
- Move docker cache to another folder [[Docker Notes]]

- Build librealsense - check `realsense-viewer` works
- 