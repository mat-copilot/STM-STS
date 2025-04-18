# STM-STS
STM_USM1600
close all
clear all
clc
% 指定.dat文件的路径
folderPath = 'F:\USM1600_STM\20250308\line\005';
% 读取文件
%dataMatrix = readmatrix(filePath,'HeaderLines', 22);
%extractedData = dataMatrix(:, [1, 3]);
% 获取该目录下所有.dat文件的信息
fileList = dir(fullfile(folderPath, '*.dat'));
% 循环遍历每个.dat文件
% 指定要读取的列
columnsToRead = [1, 3]; % 例如，读取第1列和第3列
% 初始化一个空矩阵来存储所有数据
allData = [];
for i = 1:length(fileList)
    % 构建当前文件的完整路径
    filePath = fullfile(folderPath, fileList(i).name);
    % 假设文件是文本格式，使用readmatrix读取
    data = readmatrix(filePath,'HeaderLines', 22);
    selectedData = data(:, columnsToRead);
    % 将读取的数据拼接到 allData 中
    allData = [allData; selectedData];
    % 显示读取的数据
    %disp(['Reading file: ', fileList(i).name]);
    % 这里可以添加对读取数据的处理代码，例如绘图等
    %plot(allData(:,1), allData(:,3));
end
% 指定输出文件的路径
%outputFilePath = 'combined_data.dat';

% 将拼接后的数据写入输出文件
%writematrix(allData, outputFilePath);

% 显示完成信息
%disp(['所有文件的数据已成功拼接并保存到 ', outputFilePath]);
% 添加标题和坐标轴标签
%a=reshape(allData,[401,82]);b=a(:,41:82);
a=501;
b=41;%测量的线谱数加1
newdidv=zeros(a,b+1);
newdidv(:,1)=allData(1:a,1);
for j=1:b
    newdidv(:,j+1)=allData(j*a-(a-1):j*a,2);%no offset
    %stack=10^(-11);
    %newdidv(:,j+1)=allData(j*a-500:j*a,2)+(j-1)*stack;%offset
end
% 提取第一列作为 x 轴数据
x = newdidv(:, 1);
% 提取其余列作为 y 轴数据
y = newdidv(:, 2:end);
% 绘制图形
figure; % 创建一个新的图形窗口
plot(x, y);
y_min = 0;
y_max = max(max(y)); 
ylim([y_min, y_max]);
% 添加标题和坐标轴标签
title('Pb#3006.3ds');
xlabel('Bias');
ylabel('dI/dV(a.u.)');
