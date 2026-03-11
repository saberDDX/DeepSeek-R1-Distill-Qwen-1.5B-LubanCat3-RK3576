# DeepSeek-R1-Distill-Qwen-1.5B-LubanCat3-RK3576
基于lubancat(rk3576)硬件的DeepSeek-R1-Distill-Qwen-1.5B部署，实现板卡端轻量化对话交互。

#硬件平台
-开发板：lubancat38(rk3576芯片)8+64g
-系统：Ubuntu 22.04

##具体步骤

#pc端

-下载DeepSeek-R1-Distill-Qwen-1.5B
 sudo apt update && sudo aptinstall git-lfs
 git lfs install
 git clone https://hf-mirror.com/deepseek-ai/DeepSeek-R1-Distill-Qwen-1.5B
 
-获取rknn-llm
 git clone https://github.com/airockchip/rknn-llm.git

 -搭配rkllm-toolkit环境
 conda create-n rkllm_1.1.4python=3.10
 conda activate rkllm_1.1.4

 -在新搭建的环境中安装rkllm-toolkit
  cd rknn-llm/rkllm-toolkit
  pip3 install rkllm_toolkit-1.1.4-cp310-cp310-linux_x86_64.whl

 -生成量化数据（将rknn-llm/examples/rkllm_api_demo/export中原有data_quant_json原始数据转换成DeepSeek-R1-Distill-Qwen-1.5B中的原始数据）
  cd rknn-llm/examples/rkllm_api_demo/export
  python generate_data_quant.py-m ../DeepSeek-R1-Distill-Qwen-1.5B/

  -生成rk3576适配的.rkllm模型（rknn-llm/examples/rkllm_api_demo/export/export_rkllm.py)
  先修改export_rkllm.py中的模型路径为前面拉取的deepseek-ai/DeepSeek-R1-Distill-Qwen-1.5B路径
  modelpath='/path/to/DeepSeek-R1-Distill-Qwen-1.5B'
  python export_rkllm.py
  将生成的DeepSeek-R1-Distill-Qwen-1.5B_rkllm文件中的DeepSeek-R1-Distill-Qwen-1.5B_W8A8_RK3588.rkllm替换成适配的DeepSeek-R1-Distill-Qwen-1.5B_W4A16_RK3576.rkllm
  
  #板卡端
  将修改过的DeepSeek-R1-Distill-Qwen-1.5B_rkllm上传到板卡（同局域网传输，或者用u盘拷贝）
  运行DeepSeek-R1-Distill-Qwen-1.5B_rkllm中的demo_Linux_aarch64,最终实现板卡端轻量化对话交互。
  
  #已转换的rkllm模型在https://console.box.lenovo.com/l/l0tXb8，提取码rkllm
