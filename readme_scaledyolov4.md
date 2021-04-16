#30/03/2021
### create an environment `thuong` in rank server
conda create -n thuong python=3.7
source activate thuong
### install pytorch GPU on `thuong` 
1st: https://pytorch.org/get-started/locally/

2rd: `conda install pytorch==1.7.1 torchvision==0.8.2 cudatoolkit=10.2 -c pytorch -y`
### Install mish-cuda on rank server

### install mish-cuda, if you use different pytorch version, you could try https://github.com/thomasbrandon/mish-cuda
`$ git clone https://github.com/JunnYu/mish-cuda
 $ cd mish-cuda
 $ python setup.py build install`

**ERROR: RuntimeError: Error compiling objects for extension**

**SOLVED**: Cus the difference version of pytorch so mish-cuda error then solved by using 2rd way to reinstall pytorch

### Config Scaled-YOLOv4
Clone this source code 
`$ git clone -b yolov4-large https://github.com/WongKinYiu/ScaledYOLOv4`

`$ wget https://github.com/WongKinYiu/ScaledYOLOv4/blob/yolov4-large/train.py`

Config some file to train custom dataset

ScaledYOLOv4  
  
	----data

	     --coco_person_face_filter.yaml
		    
	----models
	    
                --yolov4-p5.yaml
		    
		--yolov4-p6.yaml
		    
		--yolov4-p7.yaml
		    
		--yolov4-scp.yaml
		
Change `nc = 80` (number class) of two file .yaml in data and models to your custom class

In `coco_person_face_filter.yaml` change path of your data train, valid and test. You can using path or file .txt

Then change `names = []` with name of your class

### Train your custom Scaled-YOLOv4

From source code 

`python -m torch.distributed.launch --nproc_per_node 2 train.py --batch-size 128 --img 896 896 --data data/data.yaml --cfg models/yolov4-p5.yaml --weights '' --sync-bn --device 0,1 --name yolov4-p5 --resume`

**ERROR with batch-size not multiple with GPU**

batch-size = 64 with 4 GPU
batch-size = 32 with 2 GPU

**Train your custom data**
`
python train.py \
--batch-size 16 \
--img 320 320 \
--data data/data.yaml \
--cfg models/yolov4-p5.yaml \
--weights '' \
--sync-bn \
--device 2,3 \
--name yolov4-p5 \
--epochs 100`

While optional arguments:

  `-h, --help            show this help message and exit
  
  --weights WEIGHTS     initial weights path
  
  --cfg CFG             model.yaml path
  
  --data DATA           data.yaml path
  
  --hyp HYP             hyperparameters path, i.e. data/hyp.scratch.yaml
  
  --epochs EPOCHS
  
  --batch-size BATCH_SIZE
                        total batch size for all GPUs
			
  --img-size IMG_SIZE [IMG_SIZE ...]
                        train,test sizes
			
  --rect                rectangular training
  
  --resume [RESUME]     resume from given path/last.pt, or most recent run if blank
  
  --nosave              only save final checkpoint
  
  --notest              only test final epoch
  
  --noautoanchor        disable autoanchor check
  
  --evolve              evolve hyperparameters
  
  --bucket BUCKET       gsutil bucket
  
  --cache-images        cache images for faster training
  
  --name NAME           renames results.txt to results_name.txt if supplied
  
  --device DEVICE       cuda device, i.e. 0 or 0,1,2,3 or cpu
  
  --multi-scale         vary img-size +/- 50%
  
  --single-cls          train as single-class dataset
  
  --adam                use torch.optim.Adam() optimizer
  
  --sync-bn             use SyncBatchNorm, only available in DDP mode
  
  --local_rank LOCAL_RANK
                        DDP parameter, do not modify
			
  --logdir LOGDIR       logging directory
  `
**Train with single GPU**
python train.py --epoch 100 --batch-size 32 --img 320 320 --data data/person_ivsr.yaml --cfg models/yolov4-p5.yaml --weights '' --sync-bn --name yolov4-p5
