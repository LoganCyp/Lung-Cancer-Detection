# Lung Cancer Detection

Feed Forward Neural Network Model, the model inputs a vector of data extracted from the gray level co-occurance matrix. Then the model goes through a model consisting of the 2 hyperbolic tangent functions and 1 pure linear output layer. The test and train sets are split in a 70%-30% split. 

### Neural Network Model

```python
net = network;
net.numInputs = 1;
net.numLayers = 4;

P = size(double(c1));  
T = size(double(c2)); 
   
net = newff(P,T,[6,6]);
net.trainFcn = 'traingdx';

net.layers{1}.transferFcn = 'tansig';
net.layers{2}.transferFcn = 'tansig';
net.layers{3}.transferFcn = 'purelin';

net.divideFcn='divideint';
net.divideParam.trainRatio = 0.30;
net.divideParam.testRatio = 0.7;
net.divideParam.valRatio = 0;

net.plotFcns = {'plotperform','plottrainstate','ploterrhist'};
net.trainParam.goal = 0.001;
net.trainParam.epochs = 500;
net.trainParam.time = inf;
net.trainParam.show = 5;

[net,tr] = train(net,P,T,'CheckpointFile','weights.mat');
testInputs = P(:,tr.testInd);
testTargets = T(:,tr.testInd);
out = round(sim(net,testInputs)); 
diff = (testTargets - 2*out);
disp(diff);

perf = perform(net,P,T);
disp(perf);

detections = length(find(diff<-1));
false_positives = length(find(diff>1));
true_positives = length(find(diff<-1));
false_alarms = length(find(diff==-2));

Nt = size(testInputs,2);           
fprintf('Total testing samples: %d\n', Nt);
cm = [detections, false_positives, false_alarms, true_positives];
cm_p = (cm ./ Nt) .* 100           ;
disp(cm);
```
