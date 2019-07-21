# travis-ci 自动部署到VPS

## CI 部分
```
1.   ssh-copy-id root@IP  添加到服务器上
     ssh root@服务器的IP地址


2.  git clone 你的项目地址

3.  登录 https://travis-ci.org/ 授权travis, 并添加所需要的项目

4.  创建 .travis.yml 添加测试,编译命令等

5.  travis 对应的项目创建环境变量, 防止泄露


```

## CD 部分
```
1. 安装gem 

使用中国镜像

$ gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
$ gem sources -l
https://gems.ruby-china.com
# 确保只有 gems.ruby-china.com


# 安装travis命令行工具，如无法使用gem指令须先安装ruby
2. gem install travis


3. travis login --auto

# 此处的--add参数表示自动添加脚本到.travis.yml文件中
4. travis encrypt-file ~/.ssh/id_rsa --add
# 这个命令会自动把 id_rsa 加密传送到 .git 指定的仓库对应的 travis 中去


并且在.travis.yml里的before_install周期中自动多了下面这2行
- openssl aes-256-cbc -K $encrypted_97d432d3ed20_key -iv $encrypted_97d432d3ed20_iv
  -in id_rsa.enc -out ~\/.ssh/id_rsa -d

默认生成的命令可能会在/前面带转义符\，我们不需要这些转义符，手动删掉所有的转义符，否则可能在后面引发莫名的错误


5. 修改id_rsa权限

before_install
- openssl aes-256-cbc -K $encrypted_97d432d3ed20_key -iv $encrypted_97d432d3ed20_iv
  -in id_rsa.enc -out ~/.ssh/id_rsa -d
- chmod 600 ~/.ssh/id_rsa


6. ssh known_hosts

因为 travis-ci 默认只添加了 github.com, gist.github.com 和 ssh.github.com 为 known_hosts，rsync 执行时会提示是否添加，但是 travis-ci 里不能输入确认，所以需要将自动服务器的域名和商品添加到 known_hosts

addons:
  ssh_known_hosts: uedsky.com:1223

```

### 最后见项目里的配置,暂时发现这么多
