# Lung Cancer Detection

### Science Fair
This is my 2019 science fair project for the AHS Science Fair, code will be posted when all presentations have been completed.

Feed Forward Neural Network Model, the model inputs a vector of data extracted from the gray level co-occurance matrix. Then the model goes through a model consisting of the 2 hyperbolic tangent functions and 1 pure linear output layer. The test and train sets are split in a 70%-30% split. 

### Neural Network Model

```python
net = network;
net.numInputs = 1;
net.numLayers = 4;

x = size(double(mean));  
y = size(double(variance)); 
   
net = newff(x,y,[6,6]);
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

[net,tr] = train(net,x,y,'CheckpointFile','weights.mat');
testInputs = x(:,tr.testInd);
testTargets = y(:,tr.testInd);
out = round(sim(net,testInputs)); 
diff = (testTargets - 2*out);
disp(diff);

perf = perform(net,x,y);
disp(perf);

detections = length(find(diff<-1));
false_positives = length(find(diff>1));
true_positives = length(find(diff<-1));
false_alarms = length(find(diff==-2));

Nt = size(testInputs,2);           
fprintf('Total testing samples: %d\n', Nt);
detection_matrix = [detections, false_positives, false_alarms, true_positives];
cm_p = (detection_matrix ./ Nt) .* 100           ;
disp(cm_p);
```
### DICOM Viewer
Although in a very early stage, future stages will include metadata output, 3d view, and a choice to choose from a file hierarchy.

```python
Global I 

[filename, pathname] = uigetfile('*.dcm', 'Pick an Image');
     if isequal(filename,0) || isequal(pathname,0)
      
   warndlg('No File Is Selected','Warning');
           else
              I=dicomread(filename);
     
      imshow(I,'DisplayRange',[])
      title 'Input Image'
    end


info = dicominfo(filename)
```
