- install required packages: 

        pip install labelme && \
        pip install contextlib2 && \
        pip install IPython && \
        pip install pandas && \
        apt-get update && apt-get install ffmpeg libsm6 libxext6  -y && \
        apt-get update && apt-get install libgl1 && \
        apt-get install -y libgl1-mesa-dev && \
        apt-get install -y libglib2.0-0 &&

- Create train data: `python scripts/xml_to_csv.py -i dataset/train -o dataset/train_labels.csv`

- Create train data: `python scripts/xml_to_csv.py -i dataset/val -o dataset/val_labels.csv`

- Create training tfrecord: `python scripts/generate_tfrecord.py --csv_input=dataset/train_labels.csv  --output_path=dataset/train.record  --image_dir=dataset/train_img`

- Create validation tfrecord: `python scripts/generate_tfrecord.py --csv_input=dataset/val_labels.csv  --output_path=dataset/val.record  --image_dir=dataset/val_img`

*Note* Edit label from `scripts/generate_tfrecord.py`

        # TO-DO replace this with label map
        def class_text_to_int(row_label):
        if row_label == 'red_strawberry':
                return 1
        elif row_label == 'green_strawberry':
                return 2
        else:
                None

- visualize tfrecord `CUDA_VISIBLE_DEVICES=1 python scripts/visualize_tfrecord.py dataset/train.record dataset/labelmap.pbtxt`

- export `PYTHONPATH`: `export PYTHONPATH="/mrcnn/models/research" && export PYTHONPATH="/mrcnn/models/research/slim"`

- train model: `CUDA_VISIBLE_DEVICES=1 python models/research/object_detection/model_main.py --alsologtostderr --model_dir=training/ --pipeline_config_path=dataset/pipeline.config`