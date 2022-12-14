gnoland.md

【GO语言环境安装】
1.下载go安装包
     wget  https://go.dev/dl/go1.19.4.linux-amd64.tar.gz
2.设置执行环境变量
    sudo vi ~/.profile
        末尾添加：export PATH=$PATH:/usr/local/go/bin
    source ~/.profile
【问题解决1】
更换代理：go env -w GOPROXY=https://goproxy.cn

【gnoland test1安装部署】 
    test3测试网 至14/12也无法接水 官网公告待修复ing

1.创建密钥gnokey 账户 以及与gno交互
git clone git@github.com:gnolang/gno.git
cd ./gno
make install_gnokey

./build/gnokey generate

./build/gnokey add KEYNAME --recover

./build/gnokey list


2.与区块链交互
./build/gnokey query auth/accounts/ACCOUNT_ADDR --remote test2.gno.land:36657

获取测试币
https://gno.land/faucet

建看板
./build/gnokey maketx call KEYNAME --pkgpath "gno.land/r/boards" --func "CreateBoard" --args "BOARDNAME" --gas-fee "1000000ugnot" --gas-wanted "2000000" --broadcast true --chainid test2 --remote test2.gno.land:36657

interactive documentation: https://gno.land/r/boards?help&__func=CreateBoard

./build/gnokey query "vm/qeval" --data "gno.land/r/boards
GetBoardIDFromName(\"BOARDNAME\")" --remote test2.gno.land:36657

创建帖子
./build/gnokey maketx call KEYNAME --pkgpath "gno.land/r/boards" --func "CreateThread" --args BOARD_ID --args "Hello gno.land" --args\#file "./examples/gno.land/r/boards/example_post.md" --gas-fee 1000000ugnot --gas-wanted 2000000 --broadcast true --chainid test2 --remote test2.gno.land:36657

创建帖子评论
./build/gnokey maketx call KEYNAME --pkgpath "gno.land/r/boards" --func "CreateReply" --args "BOARD_ID" --args "1" --args "1" --args "Nice to meet you too." --gas-fee 1000000ugnot --gas-wanted 2000000 --broadcast true --chainid test2 --remote test2.gno.land:36657

./build/gnokey query "vm/qrender" --data "gno.land/r/boards
BOARDNAME/1" --remote test2.gno.land:36657

渲染页面
./build/gnokey query "vm/qrender" --data "gno.land/r/boards
gnolang"

3.开始本地gnoland节点
./build/gnokey add test1 --recover

./build/gnoland

./build/gnokey maketx addpkg test1 --pkgpath "gno.land/p/avl" --pkgdir "examples/gno.land/p/avl" --deposit 100000000ugnot --gas-fee 1000000ugnot --gas-wanted 2000000 --broadcast true --chainid test2 --remote localhost:26657

./build/gnokey maketx addpkg test1 --pkgpath "gno.land/p/avl" --pkgdir "examples/gno.land/p/avl" --deposit 100000000ugnot --gas-fee 1000000ugnot --gas-wanted 2000000 --broadcast true --chainid test2 --remote localhost:26657
