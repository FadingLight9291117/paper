# 综述：无标定(uncalibrated)图像的3D人脸重建技术

## 摘要

当前(Recently)，大量的研究聚焦于将3D数据结合(incorporation)进人脸分析以及它的应用。尽管(despite)能更加准确的表示人脸特征，然而3D人脸图像却比2D图像更加复杂。因此，人们投入(invest)了大量的努力(effort)去开发从无标定(uncalibrated)2D图像上重建3D人脸的系统。然而，2D到3D人脸重建问题是不存在的(ill-posed)，因此需要先验知识(prior knowledge)来限制解空间(solutions space)。在本工作中，我们回顾(review)了近十年中被提出的3D人脸重建的方法，聚焦于那些仅使用在不受控条件下拍摄(capture)的2D图片的方法。我们基于添加先验知识的技术，对所提出的方法进行了分类，考虑了三个主要策略(strategies)，即(namely)统计模型拟合，测光法(photometry)和深度学习，并且分别进行了回顾。此外，考虑到(given)统计的作为先验知识的3D人脸模型的相关性(relevance)，我们解释其构造过程并提供了一份最流行的、公开的(publicly)、可用的3D人脸模型的清单。在彻底(exhaustive)地研究了从2D到3D的人脸重建方法之后，我们观察到(observe)最近几年深度学习方法正在急速地发展，成为标准方法以替代(in replacement of)广泛的(widespread)统计模型拟合。与其他两种策略(strategies)不同的是，基于测光法的方法有所减少，因为相较于统计模型拟合和深度学习方法，需要强有力的基本(underlying)假设(assumptions)来限制重建的质量。这篇回顾还确定(identifiy)了当前的挑战，并为未来的研究提供了一些途径(avenues)。

*关键词*：3D人脸重建，3D人脸成像，3D形变模型



## 1. 引言

​	面部分析已经在许多不同应用中得到了广泛的应用(exploited)，包括人机交互，安全，动画(animation)，甚至是健康。在这个领域的一个最新趋势是结合(incorporate)3D数据，以克服(overcome)普遍存在的(ubiquitous)2D面部分析的一些内在(intrinsic)问题。由于人脸的3D性质(nature)，一张2D图像不足以准确地捕获人脸的几何形状(geometry)，这是因为2D图像缺失(collapse，折叠)了一个维度(dimension)。此外(furturemore)，3D成像提供了一种姿态和照明不变(invariant)的面部几何图形的表示(representation)，这就是2D成像的两个主要不便(inconvenience)之处。

​	3D人脸分析系统系统带来的有利条件以更加复杂的成像过程为代价，这通常会限制它的使用范围(scope)。3D人脸信息通常是使用立体视觉(stereo-vision)系统、3D激光(laser)扫描仪(scanner)和RGB-D摄像机(cameras)来捕获的。前两个方法可以捕获高质量(quality, quantity)的面部扫描，但是需要控制环境和昂贵的机器(machinery)。相反(in constrast)，RGB-D摄像机更廉价并且更容易使用，但是扫描结果的质量不高。

​	一种吸引人的(appealing, appeal, 呼吁，恳求，对...有吸引力)替代捕获3D人脸扫描的方法是从未标定的2D图像中估计出人脸几何形状。这一从2D到3D的人脸重建的替代方案，是为了融合捕获2D图像的简易性和人脸的3D几何形状表示的优点。

​	即使这个方法很吸引人的(attractive)，但是它还是有天生的(inherently)缺陷(ill-posed)：单个(individual)面部几何形状、头部的姿态和它的纹理(texture，质地，结构，本质)(包括照明度(illumination，照明，阐明，照明度)和颜色)都必须从一个单一的照片中恢复(recover)，这就导致(lead to)了一个未确定的(underdetermined)问题。结果(as a consequence)，从2D到3D人脸重建技术就有了一些模棱两可的问题——因为单一的2D图片可以产生不同的3D人脸，并且很难确定(determine)哪一个是正确的几何图形。

​	最近的方法进展(progress)有助于实现了非常(remarkablely, 引人注目的)令人信服(convincing, convince, 说服)的重建工作，使得在各种领域使用从2D到3D人脸重建技术成为可能。一些方法甚至能够恢复(recover)本地的细节，比如皱纹，或者重建了在极端条件下的3D人脸图像，例如遮挡(occlusion)或者较大的头部姿态。

​	从2D到3D重建方法成功的一个关键是添加一个先验知识来解决问题中的歧义问题。在最近十年，我们可以区分出三种添加先验知识的策略，也就是(namely)，统计模型拟合，光测法，以及深度学习。第一种方法，先验知识被编码进了3D面部模型中，该模型由一组3D面部扫描的集合构建，它被拟合进了输入的图像中。第二种方法，光测法结合了一个3D模板或是一个3D面部模型来估计面部表面的法线(normal)。基于这个策略的方法通常使用多张照片的信息，这进一步(further)限制(constrain)了这个问题。第三种方法，通过深度神经网来实现从2D到3D的映射，只要给定(given)适当的(appropriate)训练集，就可以学习到与几何形状和人脸外观(appearance)关联的必要先验知识。

​	在这个工作中，我们回顾了近期的从一个或更多未标定的2D图像中重建人脸的研究。对于上述三种主要的策略，我们总结(summary)、比较和讨论了近十年提出的最相关的方法。我们也会为所有提出的方法引入一个共同的数学框架，其符号在附录A中总结。
