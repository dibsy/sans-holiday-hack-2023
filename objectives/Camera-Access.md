## Objective
Gain access to Jack's camera. What's the third item on Jack's TODO list?

## Solution

```bash
sudo docker cp cebd83b7652e:/opt/nmf/cli-tool/out.txt .
```
```bash
base64 -d out.txt.proc2 > img.proc2
```
```bash
file img.proc2               
img.proc2: JPEG image data, JFIF standard 1.01, aspect ratio, ...
```
