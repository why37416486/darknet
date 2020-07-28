1. 将图片放置到 darknet/VOCdevkit/VOC2007/JPEGImages目录底下
2. 将图片的标签放置在 darknet/VOCdevkit/VOC2007/Annotations目录底下
3. 在 darknet/VOCdevkit/VOC2007 路径底下运行 python test.py 生成图片名字
4. 在路径 darknet/VOCdevkit 底下运行 python voc_label.py 数据清洗
5. 在路径 darknet/VOCdevkit 底下运行 cat 2007_train.txt 2007_val.txt  > train.txt 集合文件的绝对路径
6. 返回到 darknet 目录底下运行 vi cfg/yolov3-voc.cfg 看需求修改下面标注的地方（这个一共有三处需要修改，建议从最底下开始修改）<br>
	[convolutional]
	size=1
	stride=1
	pad=1
	filters=21		//修改为filters=(classes+5)x3
	activation=linear

	[yolo]
	mask = 0,1,2
	anchors = 10,13,  16,30,  33,23,  30,61,  62,45,  59,119,  116,90,  156,198,  373,326
	classes=2		//修改为需要检测的类别数目
	num=9
	jitter=.3
	ignore_thresh = .5
	truth_thresh = 1
	random=1	//当显卡内存不够时可以修改为0

7. 在 darknet 目录下运行 vi cfg/voc.data 看需求修改下面标注的地方
	classes= 2	//类别数目
	train  = /home/ustc/Kaixiang/YOLO/darknet/scripts/2007_train.txt	//训练集目录
	valid  = /home/ustc/Kaixiang/YOLO/darknet/scripts/2007_val.txt	//验证集目录
	names = data/voc.names	//类别名称目录
	backup = backup		//模型的存储目录
8. 在 darknet 目录下运行 vi Makefile 看需求修改下面标注的地方（每一次修改Makefile文件都要重新运行 make 编译，其他文件则不用）
	GPU=1    #如果使用GPU设置为1，CPU设置为0
	CUDNN=1  #如果使用CUDNN设置为1，否则为0
	OPENCV=1 #如果调用摄像头，还需要设置OPENCV为1，否则为0
	OPENMP=0 #如果使用OPENMP设置为1，否则为0
	DEBUG=0  #如果使用DEBUG设置为1，否则为0
	nvcc=/usr/local/cuda-10.0/bin/nvcc
9. 在 darknet 目录下运行 vi data/voc.names 添加你所分类的名字
10. 在 darknet 目录下运行 ./darknet detector train cfg/voc.data cfg/yolov3-voc.cfg darknet53.conv.74 | tee log.txt 开始训练，在当前目录下会生成log.txt文件，里面存放着训练时的输出数据，用于评估训练出来的模型。
11. 在训练的过程中，会在 darknet/backup 文件夹底下产生一些权重模型，其中 .backup 为后缀的文件为最新的权重文件
12. 可以运行 ./darknet detector train cfg/voc.data cfg/yolov3-voc.cfg backup/yolov3-voc.backup 继续训练
13. 训练结束后，在 darknet 目录底下运行 ./darknet detector test cfg/voc.data cfg/yolov3-voc.cfg 权重文件 要检测图片的路径（例 ./darknet detector test cfg/coco.data cfg/yolov3-voc.cfg backup/yolov3-voc_final.weights data/test.jpg）进行测试<br>
14. 图片检测 ./darknet detector test cfg/coco.data cfg/yolov3-voc.cfg backup/yolov3-voc_final.weights data/test.jpg<br>
15. 视频检测 ./darknet detector demo cfg/coco.data cfg/yolov3-voc.cfg backup/test data/input.avi<br>
	./darknet detector demo data/voc.data yolov3.cfg yolov3.weights data/3.mp4 -out_filename xxx.mp4<br>
16. 参考： <br>
	错误集锦 https://www.twblogs.net/a/5b8e0cee2b7177188342863e/zh-cn <br>
	配置文件 https://blog.csdn.net/hrsstudy/article/details/65447947 <br>
	yolov3-voc.cfg 文件说明 https://blog.csdn.net/maizousidemao/article/details/103541490 <br>
	log信息 https://blog.csdn.net/maweifei/article/details/81148414 <br>
	darknet/cfg/yolov3-voc.cfg 信息 https://github.com/pjreddie/darknet/commit/e84933bfdd7315736c442a41d9aed163843dda54#diff-78bdd0cc6cd80cfee41f72e28b53d14e <br>
	综合 https://github.com/AlexeyAB/darknet#how-to-train-pascal-voc-data<br>
	理解 https://github.com/YunYang1994/cv-notebooks/blob/master/ai_algorithm/YOLOv3.md <br>
	修改结果图片 https://www.cnblogs.com/luzeming/p/10657823.html <br>
	添加置信度 https://blog.csdn.net/MichalCong/article/details/91050167 <br>
	保存识别的视频 https://blog.csdn.net/sinat_33718563/article/details/79964758?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2 <br>
	详细 https://www.cnblogs.com/luzeming/p/10657823.html <br>
	car		机器人<br>
	base		泉水本体 <br>
	armor		甲板 <br>
	watcher		哨兵 <br>
	ignore		忽视 <br>
