# track_sequence_anomaly_detection
由时间空间成对组成的轨迹序列，通过循环神经网络，自编码器，时空密度聚类完成异常检测

#####  1.rnn
&emsp; 通过rnn预测下一个时空点的概率分布，计算和实际概率分布的kl离散度

&emsp; 执行代码
```
python data_preprocessing.py
python data_loader.py
python pre_embedding.py
python predict.py
```

&emsp; 轨迹数据格式：hour_location hour_location hour_location ...
```
[1] 6_10101 7_10094 8_10096 9_10102 10_10097
[2] 6_10094 8_10103 10_10101 11_10103 11_10095 14_10097
[3] 12_10094 12_10094 12_10097 13_10096 13_10094 14_10097 16_10096 18_10102
```

&emsp;轨迹检测
```
请输入轨迹序列:7_10106 7_10096 8_10095 8_10095 8_10100 9_10102 11_10101 11_10104 12_10103 12_10104 12_10097 13_10097 14_10094 15_10095 15_10103 15_10104 16_10097 17_10094 17_10094 17_10096 17_10106 17_10092 17_10102 17_10094 18_10092 18_10101 18_10101 21_10095 22_10104 22_10104 
[ ] 交叉熵：7.609000205993652, kl离散度：7.873, 移动轨迹：['7_10106'] => 7_10096
[ ] 交叉熵：7.607999801635742, kl离散度：8.403, 移动轨迹：['7_10106', '7_10096'] => 8_10095
[ ] 交叉熵：7.60699987411499, kl离散度：8.662, 移动轨迹：['7_10106', '7_10096', '8_10095'] => 8_10095
[ ] 交叉熵：7.572999954223633, kl离散度：9.714, 移动轨迹：['7_10106', '7_10096', '8_10095', '8_10095'] => 8_10100
[ ] 交叉熵：7.599999904632568, kl离散度：10.423, 移动轨迹：['7_10096', '8_10095', '8_10095', '8_10100'] => 9_10102
[ ] 交叉熵：7.598999977111816, kl离散度：9.946, 移动轨迹：['8_10095', '8_10095', '8_10100', '9_10102'] => 11_10101
[ ] 交叉熵：7.5289998054504395, kl离散度：9.099, 移动轨迹：['8_10095', '8_10100', '9_10102', '11_10101'] => 11_10104
[ ] 交叉熵：7.59499979019165, kl离散度：10.283, 移动轨迹：['8_10100', '9_10102', '11_10101', '11_10104'] => 12_10103
[ ] 交叉熵：7.520999908447266, kl离散度：8.919, 移动轨迹：['9_10102', '11_10101', '11_10104', '12_10103'] => 12_10104
[ ] 交叉熵：7.453000068664551, kl离散度：8.242, 移动轨迹：['11_10101', '11_10104', '12_10103', '12_10104'] => 12_10097
[ ] 交叉熵：7.599999904632568, kl离散度：10.468, 移动轨迹：['11_10104', '12_10103', '12_10104', '12_10097'] => 13_10097
[ ] 交叉熵：7.567999839782715, kl离散度：9.526, 移动轨迹：['12_10103', '12_10104', '12_10097', '13_10097'] => 14_10094
[×] 交叉熵：7.609000205993652, kl离散度：10.895, 移动轨迹：['12_10104', '12_10097', '13_10097', '14_10094'] => 15_10095
[ ] 交叉熵：7.5970001220703125, kl离散度：9.827, 移动轨迹：['12_10097', '13_10097', '14_10094', '15_10095'] => 15_10103
[ ] 交叉熵：7.546000003814697, kl离散度：9.446, 移动轨迹：['13_10097', '14_10094', '15_10095', '15_10103'] => 15_10104
[ ] 交叉熵：7.548999786376953, kl离散度：9.49, 移动轨迹：['14_10094', '15_10095', '15_10103', '15_10104'] => 16_10097
[ ] 交叉熵：7.585999965667725, kl离散度：10.284, 移动轨迹：['15_10095', '15_10103', '15_10104', '16_10097'] => 17_10094
[ ] 交叉熵：7.455999851226807, kl离散度：8.381, 移动轨迹：['15_10103', '15_10104', '16_10097', '17_10094'] => 17_10094
[ ] 交叉熵：7.570000171661377, kl离散度：9.628, 移动轨迹：['15_10104', '16_10097', '17_10094', '17_10094'] => 17_10096
[ ] 交叉熵：7.505000114440918, kl离散度：9.048, 移动轨迹：['16_10097', '17_10094', '17_10094', '17_10096'] => 17_10106
[ ] 交叉熵：7.60099983215332, kl离散度：10.484, 移动轨迹：['17_10094', '17_10094', '17_10096', '17_10106'] => 17_10092
[ ] 交叉熵：7.561999797821045, kl离散度：9.993, 移动轨迹：['17_10094', '17_10096', '17_10106', '17_10092'] => 17_10102
[ ] 交叉熵：7.571000099182129, kl离散度：10.175, 移动轨迹：['17_10096', '17_10106', '17_10092', '17_10102'] => 17_10094
[×] 交叉熵：7.607999801635742, kl离散度：10.662, 移动轨迹：['17_10106', '17_10092', '17_10102', '17_10094'] => 18_10092
[ ] 交叉熵：7.5229997634887695, kl离散度：9.108, 移动轨迹：['17_10092', '17_10102', '17_10094', '18_10092'] => 18_10101
[ ] 交叉熵：7.46999979019165, kl离散度：8.728, 移动轨迹：['17_10102', '17_10094', '18_10092', '18_10101'] => 18_10101
[×] 交叉熵：7.609000205993652, kl离散度：10.609, 移动轨迹：['17_10094', '18_10092', '18_10101', '18_10101'] => 21_10095
[ ] 交叉熵：7.590000152587891, kl离散度：9.594, 移动轨迹：['18_10092', '18_10101', '18_10101', '21_10095'] => 22_10104
[ ] 交叉熵：7.578000068664551, kl离散度：9.727, 移动轨迹：['18_10101', '18_10101', '21_10095', '22_10104'] => 22_10104
```