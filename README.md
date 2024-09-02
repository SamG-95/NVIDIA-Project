VIDEO: [https://www.youtube.com/watch?v=kJ4-O_P8PzY](https://youtu.be/JmQ0e_JwTic?si=9a-CCot-pySrd4vy)

Hello, 
My .onnx file was too big, so here's a link to it: https://drive.google.com/file/d/1j6Pq_DxVMUfHQNypcFeDcWnUlS9ZZul3/view?usp=sharing

Here is the full type_ds dataset shown off in the video: https://drive.google.com/file/d/1YKTVv-XfZV-MyAtDX6hEWaHr9g32beGD/view?usp=sharing

INSTRUCTIONS THAT I USED (Apparently you can do it easier using the .onnx from the start):

1a. Extract the type_ds dataset, and move it into 
> jetson-inference/python/training/classification/data

1b. Download the train.py file, and move it into
> jetson-inference/python/training/classification

2. On your nano, change directories into 
> jetson-inference/python/training/classification/data

3. From the jetson-inference folder, run 
> ./docker/run.sh to run the docker container.

4. Change directories so you are in 
> jetson-inference/python/training/classification

5. Run the training script to re-train the network where the model-dir argument is where the model should be saved and where the data is.  You should immediately start to see output, but it will take a very long time to finish running. 
> python3 train.py --model-dir=models/type_ds data/type_ds

6. Once finished, run the onnx export script.
> python3 onnx_export.py --model-dir=models/type_ds

7. Exit the docker container by pressing Ctl + D.

8. Set the NET and DATASET variables
> NET=models/type_ds
> DATASET=data/type_ds

9. Create output folders
> mkdir $DATASET/test_output_3dsxl $DATASET/test_output_n3dsxl $DATASET/test_output_n2dsxl $DATASET/test_output_dsixl

10. Use your model on every image in your test folder
> imagenet --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt \
           $DATASET/test/'New 3DS XL' $DATASET/test_output_n3dsxl
           
> imagenet --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt \
           $DATASET/test/'New 2DS XL' $DATASET/test_output_n2dsxl
           
> imagenet --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt \
           $DATASET/test/'3DS XL' $DATASET/test_output_3dsxl
           
> imagenet --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt \
           $DATASET/test/'DSI XL' $DATASET/test_output_dsixl


