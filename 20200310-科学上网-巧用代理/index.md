# 科学上网-巧用镜像站


## git clone 代理
```
git config --global http.proxy http://127.0.0.1:10809
git config --global https.proxy https://127.0.0.1:1081
# 查看代理配置
git config --global -e
# 取消代理
git config --global --unset http.proxy

```
