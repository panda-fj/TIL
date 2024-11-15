# コンソールの実行

```py
result = subprocess.run("command here", shell=True)
print(result)
```

`subprocess.run`の第一引数にコマンドを入れる。  

## 例：ffmpegを実行するとき

```py
command = f"ffmpeg -i \"{url}\" \"{title}.mp3\""
result = subprocess.run(command, shell=True)
print(result)
```