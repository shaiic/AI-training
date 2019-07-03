##### Q1: 在看图识熊的实验中，如何导出训练模型？

**A:** 在[看图识熊](1.Notebooks/4.Lesson-Docs/lesson04-深度学习.01-定制化(看图识熊)/Bear-Recognition.md)这篇文章中，同学们可以找到如何导出训练模型。目前支持对四种模型的导出，对三大操作系统都	 能支持，这里我们选择ONNX。

​	ONNX，全称Open Neural Network Exchange，即开放神经网络交换格式，是由微软、Facebook等多个相关	公司一起推动的深度学习模型标准。

​	选择ONNX后，在Choose a version下拉框中选择ONNX1.2，点击Export，等待服务器把模型导出后，然后点	击Download，即可下载模型到本地。**注意！存放模型文件的路径不要包含中文。**

##### Q2: 在看图识熊的实验中，如何实现对模型识别API的调用？

**A:** 为了构建本地模型识别的离线应用，在[看图识熊](1.Notebooks/4.Lesson-Docs/lesson04-深度学习.01-定制化(看图识熊)/Bear-Recognition.md)这篇文章中，同学们可以找到如何导出训练模型。通常在下载	 的模型ZIP文件中包含有Sample code，同学们可以在那里面找到相应的API调用实例。

##### Q3: 在看图识熊的目标检测中，Customvision中object detection全部做好导出来，到Visual Studio 2017中		改下输出文件名称，但走不通。接下来应该怎么做?

**A:** 老师在课堂上的举例是图像分类，Project type选择的是Classification。图像分类问题的输出为各个类别标签，	 以及属于这个类别的概率。目标检测的实验中，Project type是object detection，输出为图像中存在超过指定	 阈值的物体标签，及其所在位置坐标和概率。这两个输出不一样，仅仅改文件名称当然走不通，需要对代码进	 行修改。

##### Q4: 在智能家居的实验中，对于实体(Entity)和意向(Intent)的构造，有没有什么这方面的原则指导？包括对多		个实体的设置有什么最佳实践？

**A:** 请参考《[使用认知服务生成语言理解应用的最佳做法](https://docs.microsoft.com/zh-cn/azure/cognitive-services/LUIS/luis-concept-best-practices)》。 

##### Q5: 在智能家居的实验中，如果存在多盏灯的控制，例如“把所有灯都关掉”，实体和意向的设置如何考虑？

**A:**  实体和意向的判断有比较大的开放度，在本例当中，“所有灯都关掉”，可以是意向，也可以把所有灯作为实体	  重要的是你自己设计的系统根据你的设置可以实现就行。本题中“把所有灯都关掉”作为意向相对简单一些。