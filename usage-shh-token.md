[TOC]
# SSH
**绑定主机，在设置的权限下免密操作github**，[网上说明](https://blog.csdn.net/weixin_42310154/article/details/118340458)

## 配置
1.  检查本地是否存在ssh key，即是否存在 id_rsa 和 id_rsa 文件，是的话跳第3步
    ```
    $ ls ~/.ssh
    > config  id_rsa  id_rsa.pub  known_hosts  known_hosts.old
    ```

2.  生成ssh key，使用git的user email
    ```
    ssh-keygen -t rsa -C "<git config user.email>"
    ```

3.  获取ssh key公钥内容(id_rsa.pub)
    ```
    $ cat ~/.ssh/id_rsa.pub
    > ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDRiSm2d5csR35Ll8DdvzIhLWZpE5XbhtohnpCj960c66Go9/UkaT4zu/J42adXsKQVR8SCthW8aUrhaD0TT4GstyhQfPjWLxTKGKqrTeS/p2PtT6jKAfLdfakcPKnFTWreTlvKi09on5UCE/nDsNx8Se+HWidtf+8Ep58v81MBDMAeIXpwoA8Z7bWw3weJ0/XXXskXfID5NwNaA6ZErH/Vlo9l/Umjz331YDPCOaHa28K962YwJhLpX86r4RczDwbfwKL/ClXJ72kc0ae5CMcjkQSiIRCuqctT/hG3pyZEzkRuE2qVUxczPoNjNY8Hp3M19FLuGZNdTsJFdv10Q2/Vk3wjdV15o4FCXyU/KqDsFW7XAyRsbhBcLcw7ncZEGmdbUQmA1RWcMmIjYaaEwfjBZF1xvpLS2vtCiIZ12rS6LCINSkO5qkTDpG5ZX1lUPD8EmXpquSUTfuraPOohNOgdbhKJppZLTuRmifYBenNF/p0n4+alrJtKTFXK/Cn1qAU= c130311@SOA0088
    ```

4.  Github账号上添加公钥
    `setting` → `SSH and GPG keys` → `New SSH key`
    ![](img/2024-03-22-15-53-04.png)

5.  验证是否设置成功
    ```
    $ ssh -T git@github.com
    > Hi HongJiong! You've successfully authenticated, but GitHub does not provide shell access.
    ```

## 注意之后在clone仓库的时候要使用ssh的url，而不是http

---

# Token
用http的url时，会出现错误，且要求输入账号密码，其实没什么鸟用，原因是github要求对所有经过身份验证的Git操作要求使用基于令牌的身份验证(例如，个人访问GitHub应用程序安装令牌)，不再接受账户密码。[网上说明](https://blog.csdn.net/qq_37255976/article/details/134558484)，[官方说明](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)s

1.  配置入口
    ![](img/2024-03-22-16-11-20.png)

2.  配置
    ![](img/2024-03-22-16-13-57.png)

3.  生成后的令牌码只出现一次，除非重新生成，记得保存
    ![](img/2024-03-22-16-16-07.png)

4.  使用
    - `git remote set-url origin https://<your_token>@github.com/<USERNAME>/<REPO>.git`
      - <your_token>：生成的Token
      - \<USERNAME>：你的username
      - \<REPO>：要访问仓库的名称
      - 
        ```
        git remote set-url origin https://HongJiong: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@github.com/HongJiong/UsageOfGit.git
        ```
    
    - 若是新仓库
      ```
      $ git clone https://github.com/<USERNAME>/<REPO>.git
      > Username: your_username
      > Password: your_token
      ```
    
    - 若是已有仓库
      `$ git remote set-url origin https://<your_token>@github.com/<USERNAME>/<REPO>.git`
