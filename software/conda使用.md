# conda

```bash
# 新建环境
conda create -n <环境名>  python=3.8
#进入环境
conda activate <环境名>
#查看环境下的软件
conda list

#安装软件
conda install <软件包>
#卸载软件
conda remove <软件包>
#更新软件
conda update <软件包>
# 更新conda，保持conda最新
conda update conda

#退出环境
conda deactivate 

#查看环境
conda env list
conda info -e
#删除环境
conda remove -n <环境名> --all


```

