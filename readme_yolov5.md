## Same with Scaled-YoLov4
1. Clone YOLOv5 repository
'''
$ !git clone https://github.com/ultralytics/yolov5  # clone repo
$ %cd yolov5
$ !git reset --hard 886f1c03d839575afecb059accf74296fad395b6
'''

2. Change file data in data/*.yaml

3. Change **nc = 80** in models/yolov5s.yaml

4. Start train model 

**Train on multiple GPU**

`python train.py --img 320 --batch 32 --epochs 200 --data 'data/person_ivsr_test.yaml' --cfg ./models/person_yolov5s.yaml --weights '' --name yolov5s_test_results  --cache`

**Train on a GPU**

`python -m torch.distributed.launch --nproc_per_node 2 train.py --batch-size 32 --img 320 --epoch 200 --data 'data/coco_person.yaml' --cfg ./models/person_face_yolov5s.yaml --weights '' --sync-bn --device 4,5 --name personface_yolov5`



cp -avr /mnt/hdd10tb/Users/phai/datasets/ /home/thuongnh/datasets/

usage: test.py [-h] [--weights WEIGHTS [WEIGHTS ...]] [--data DATA]
               [--batch-size BATCH_SIZE] [--img-size IMG_SIZE]
               [--conf-thres CONF_THRES] [--iou-thres IOU_THRES] [--task TASK]
               [--device DEVICE] [--single-cls] [--augment] [--verbose]
               [--save-txt] [--save-hybrid] [--save-conf] [--save-json]
               [--project PROJECT] [--name NAME] [--exist-ok]

optional arguments:
  -h, --help            show this help message and exit
  --weights WEIGHTS [WEIGHTS ...]
                        model.pt path(s)
  --data DATA           *.data path
  --batch-size BATCH_SIZE
                        size of each image batch
  --img-size IMG_SIZE   inference size (pixels)
  --conf-thres CONF_THRES
                        object confidence threshold
  --iou-thres IOU_THRES
                        IOU threshold for NMS
  --task TASK           train, val, test, speed or study
  --device DEVICE       cuda device, i.e. 0 or 0,1,2,3 or cpu
  --single-cls          treat as single-class dataset
  --augment             augmented inference
  --verbose             report mAP by class
  --save-txt            save results to *.txt
  --save-hybrid         save label+prediction hybrid results to *.txt
  --save-conf           save confidences in --save-txt labels
  --save-json           save a cocoapi-compatible JSON results file
  --project PROJECT     save to project/name
  --name NAME           save to project/name
  --exist-ok            existing project/name ok, do not increment

`python test.py \
--img 320 \
--conf 0.001 \
--batch 8 \
--device 4,5 \
--data data/satutora_person_face.yaml \
--weights runs/train/personface_yolov55/weights/best.pt`

##Export model olov5 to ONNX 
`python models/export.py --weights coco_320/train/personface_yolov55/weights/best.pt --img 320 --batch 16`

## Finetune on coco dataset
python -m torch.distributed.launch --nproc_per_node 2 train.py --batch-size 32 --img 480 --epoch 200 --data 'path-to-your-data.yaml' --cfg ./models/person_face_yolov5s.yaml --weights 'path-to-your-base-model/best.pt' --sync-bn --device 4,5 --name name-you-want-to-set

## Test your model

python test.py \
        --weights path-to-weights-path/best.pt \
        --data /data/coco_person_face.yaml \
        --batch 64 \
        --img 320 \
        --device 0 \
        --verbose \
        --task 'test'\
        --conf-thres 0.001 \ 
	--single-cls \

## RESUME WHEN TRAINING
python train.py --resume ./runs/train/weights/last.pt --device 0

## DETECT VIDEO 
python detect.py --source path-to-video --weights ./runs/train/weights/best.pt --conf 0.25 --device 0 --save-txt

