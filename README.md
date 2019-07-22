# MKP
clc;  %清除所有
clear all; %清除变量
close all; %关闭图片
Capacity= [104 72]; % 背包的容量
% Weight= [4 4;4 4;4 4;4 4;4 4;4 4;4 4;4 4;4 4;4 4;4 4;4 4;4 4;4 4;4 4;4 4;4 4;...
%                   4 5;4 5;4 5;4 5;4 5;4 5;4 5;4 5;4 5;4 5;4 5;4 5;4 5;...
%                   5 4;5 4;5 4;5 4;5 4;5 4;5 4;5 4;5 4;5 4;5 4;5 4;5 4;5 4;5 4;5 4;5 4;...
%                   5 5;5 5;5 5;5 5;5 5;5 5;5 5;5 5;5 5;5 5;5 5;5 5;5 5;...
%                   5 10;5 10;5 10;5 10;5 10;5 10;...
%                   5 12;5 12;5 12;5 12;5 12;...
%                   6 10;6 10;6 10;6 10;6 10;6 10;...
%                   6 12;6 12;6 12;6 12;6 12]; 
Weight= [8 0;8 0;8 0;8 0;8 0;8 0;8 0;8 0;8 0;8 0;8 0;8 0;...
        ...4 5;4 5;4 5;4 5;4 5;4 5;4 5;4 5;4 5;4 5;4 5;4 5;4 5;... 
        0 10;0 10;0 10;0 10;0 10;0 10;...
        15 0;15 0;15 0;15 0;15 0;15 0;...
        5 12;5 12;5 12;5 12;5 12;...
        10 6;10 6;10 6;10 6;10 6;10 6;...
        0 18;0 18;0 18 ]; % 背包的装载方式 。
NumberOfObject =length(Weight); % n为背包的种类
Value= zeros(NumberOfObject,1)+1;% 背包对应的数量。
% 背包对应的数量。
TransferMatrix=[]; % 定义状态转移矩阵
ObjectState=[]; % 背包的状态
%1.判断第一个背包放或不放；
for FlagTemp2=1:Capacity(2)
        for FlagTemp1=1:Capacity(1)
                if Weight(NumberOfObject,1)<FlagTemp1&&Weight(NumberOfObject,2)<FlagTemp2
                        TransferMatrix(NumberOfObject,FlagTemp1,FlagTemp2)=Value(NumberOfObject) ;
                else
                        TransferMatrix(NumberOfObject,FlagTemp1,FlagTemp2)=0;
                end
        end
end
%2.判断下一个背包是放还是不放；不放时：M[i,u,v,w]=M[i-1,u,v,w] ；放时： M[i,u,v,w]= M[i-1,u-ui,v-vi,w-wi]+Wi；
%3.重复2.
for FlagTempExternal=NumberOfObject-1:-1:1
        for FlagTemp2=1:Capacity(2)
                for FlagTemp1=1:Capacity(1)
                        if Weight(FlagTempExternal,1)>=FlagTemp1 || Weight(FlagTempExternal,2)>=FlagTemp2
                                TransferMatrix(FlagTempExternal,FlagTemp1,FlagTemp2)=TransferMatrix(FlagTempExternal+1,FlagTemp1,FlagTemp2);
                        else
                                if TransferMatrix(FlagTempExternal+1,FlagTemp1,FlagTemp2)>TransferMatrix(FlagTempExternal+1,FlagTemp1-Weight(FlagTempExternal,1),FlagTemp2-Weight(FlagTempExternal,2))+Value(FlagTempExternal)
                                        TransferMatrix(FlagTempExternal,FlagTemp1,FlagTemp2)=TransferMatrix(FlagTempExternal+1,FlagTemp1,FlagTemp2);
                                else
                                        TransferMatrix(FlagTempExternal,FlagTemp1,FlagTemp2)=TransferMatrix(FlagTempExternal+1,FlagTemp1-Weight(FlagTempExternal,1),FlagTemp2-Weight(FlagTempExternal,2))+Value(FlagTempExternal);

                                end
                        end
                end
        end
end
% TransferMatrix
%4.找出这些背包。 
FlagTempExternal=Capacity;
for FlagTemp=1:NumberOfObject-1        
if TransferMatrix(FlagTemp,FlagTempExternal(1),FlagTempExternal(2))==TransferMatrix(FlagTemp+1,FlagTempExternal(1),FlagTempExternal(2))
                ObjectState(FlagTemp)=0;
        else
                ObjectState(FlagTemp)=1;
               FlagTempExternal=FlagTempExternal-Weight(FlagTemp,:);
        end
end
if TransferMatrix(NumberOfObject,FlagTempExternal)==0
        ObjectState(NumberOfObject)=0;
else
        ObjectState(NumberOfObject)=1;
end
disp '问题二装载方式：'
ObjectState;
ObjectCar=Weight(find(ObjectState==1),:);
CarEquat=ObjectCar
disp '问题二各类型背包数量：'
[Car,Total]=QuestionTwoSelect(ObjectCar)
