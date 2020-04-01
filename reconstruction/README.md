# Train
## Teacher

To train the teacher model: `sh train_teacher.sh`
```bash
## Teacher

BASE_PATH=''
MODEL='attention_imitation'
DATASET_TYPE='mrbrain' ##cardiac,knee
MODEL_TYPE='teacher'
ACC_FACTOR='4x' ##5x,8x

BATCH_SIZE=4
NUM_EPOCHS=150
DEVICE='cuda:0'

EXP_DIR=${BASE_PATH}'/experiments/'${DATASET_TYPE}'/acc_'${ACC_FACTOR}'/'${MODEL}'_'${MODEL_TYPE}
TRAIN_PATH=${BASE_PATH}'/datasets/'${DATASET_TYPE}'/train/acc_'${ACC_FACTOR}
VALIDATION_PATH=${BASE_PATH}'/datasets/'${DATASET_TYPE}'/validation/acc_'${ACC_FACTOR}
USMASK_PATH=${BASE_PATH}'/KD-MRI/us_masks/'${DATASET_TYPE}

echo python train_base_model.py --batch-size ${BATCH_SIZE} --num-epochs ${NUM_EPOCHS} --device ${DEVICE} --exp-dir ${EXP_DIR} --train-path ${TRAIN_PATH} --validation-path ${VALIDATION_PATH} --acceleration_factor ${ACC_FACTOR} --dataset_type ${DATASET_TYPE} --usmask_path ${USMASK_PATH} --model_type ${MODEL_TYPE}
python train_base_model.py --batch-size ${BATCH_SIZE} --num-epochs ${NUM_EPOCHS} --device ${DEVICE} --exp-dir ${EXP_DIR} --train-path ${TRAIN_PATH} --validation-path ${VALIDATION_PATH} --acceleration_factor ${ACC_FACTOR} --dataset_type ${DATASET_TYPE} --usmask_path ${USMASK_PATH} --model_type ${MODEL_TYPE}
```

## Student

To train the student model: `sh train_student.sh`
```bash
BASE_PATH=''
MODEL='attention_imitation'
DATASET_TYPE='mrbrain' ##cardiac,knee
MODEL_TYPE='student'
ACC_FACTOR='4x'##5x,8x


BATCH_SIZE=4
NUM_EPOCHS=150
DEVICE='cuda:0'

EXP_DIR=${BASE_PATH}'/experiments/'${DATASET_TYPE}'/acc_'${ACC_FACTOR}'/'${MODEL}'_'${MODEL_TYPE}
TRAIN_PATH=${BASE_PATH}'/datasets/'${DATASET_TYPE}'/train/acc_'${ACC_FACTOR}
VALIDATION_PATH=${BASE_PATH}'/datasets/'${DATASET_TYPE}'/validation/acc_'${ACC_FACTOR}
USMASK_PATH=${BASE_PATH}'/KD-MRI/us_masks/'${DATASET_TYPE}

echo python train_base_model.py --batch-size ${BATCH_SIZE} --num-epochs ${NUM_EPOCHS} --device ${DEVICE} --exp-dir ${EXP_DIR} --train-path ${TRAIN_PATH} --validation-path ${VALIDATION_PATH} --acceleration_factor ${ACC_FACTOR} --dataset_type ${DATASET_TYPE} --usmask_path ${USMASK_PATH} --model_type ${MODEL_TYPE}
python train_base_model.py --batch-size ${BATCH_SIZE} --num-epochs ${NUM_EPOCHS} --device ${DEVICE} --exp-dir ${EXP_DIR} --train-path ${TRAIN_PATH} --validation-path ${VALIDATION_PATH} --acceleration_factor ${ACC_FACTOR} --dataset_type ${DATASET_TYPE} --usmask_path ${USMASK_PATH} --model_type ${MODEL_TYPE}
```

## Feature 

To train student feature maps through teacher supervision: `sh train_feature.sh`
```bash
BASE_PATH=''
MODEL='attention_imitation'
DATASET_TYPE='mrbrain'##cardiac,knee
MODEL_TYPE='feature'
ACC_FACTOR='4x'##5x,8x

BATCH_SIZE=4
NUM_EPOCHS=150
DEVICE='cuda:0'

EXP_DIR=${BASE_PATH}'/experiments/'${DATASET_TYPE}'/acc_'${ACC_FACTOR}'/'${MODEL}'_'${MODEL_TYPE}
TRAIN_PATH=${BASE_PATH}'/datasets/'${DATASET_TYPE}'/train/acc_'${ACC_FACTOR}
VALIDATION_PATH=${BASE_PATH}'/datasets/'${DATASET_TYPE}'/validation/acc_'${ACC_FACTOR}
USMASK_PATH=${BASE_PATH}'/KD-MRI/us_masks/'${DATASET_TYPE}
TEACHER_CHECKPOINT=${BASE_PATH}'/experiments/'${DATASET_TYPE}'/acc_'${ACC_FACTOR}'/'${MODEL}'_teacher/best_model.pt'

echo python train_feature.py --batch-size ${BATCH_SIZE} --num-epochs ${NUM_EPOCHS} --device ${DEVICE} --exp-dir ${EXP_DIR} --train-path ${TRAIN_PATH} --validation-path ${VALIDATION_PATH} --acceleration_factor ${ACC_FACTOR} --dataset_type ${DATASET_TYPE} --usmask_path ${USMASK_PATH} --teacher_checkpoint ${TEACHER_CHECKPOINT}
python train_feature.py --batch-size ${BATCH_SIZE} --num-epochs ${NUM_EPOCHS} --device ${DEVICE} --exp-dir ${EXP_DIR} --train-path ${TRAIN_PATH} --validation-path ${VALIDATION_PATH} --acceleration_factor ${ACC_FACTOR} --dataset_type ${DATASET_TYPE} --usmask_path ${USMASK_PATH} --teacher_checkpoint ${TEACHER_CHECKPOINT}
```

### KD

To train kd model using pretrained student model: `sh train_kd.sh`
```bash
BASE_PATH=''
MODEL='attention_imitation'
DATASET_TYPE='mrbrain'##cardiac,knee
MODEL_TYPE='kd'
ACC_FACTOR='4x'##5x,8x

BATCH_SIZE=4
NUM_EPOCHS=150
DEVICE='cuda:0'

EXP_DIR=${BASE_PATH}'/experiments/'${DATASET_TYPE}'/acc_'${ACC_FACTOR}'/'${MODEL}'_'${MODEL_TYPE}
TRAIN_PATH=${BASE_PATH}'/datasets/'${DATASET_TYPE}'/train/acc_'${ACC_FACTOR}
VALIDATION_PATH=${BASE_PATH}'/datasets/'${DATASET_TYPE}'/validation/acc_'${ACC_FACTOR}
USMASK_PATH=${BASE_PATH}'/KD-MRI/us_masks/'${DATASET_TYPE}
TEACHER_CHECKPOINT=${BASE_PATH}'/experiments/'${DATASET_TYPE}'/acc_'${ACC_FACTOR}'/'${MODEL}'_teacher/best_model.pt'
STUDENT_CHECKPOINT=${BASE_PATH}'/experiments/'${DATASET_TYPE}'/acc_'${ACC_FACTOR}'/'${MODEL}'_feature/best_model.pt'

echo python train_kd.py --batch-size ${BATCH_SIZE} --num-epochs ${NUM_EPOCHS} --device ${DEVICE} --exp-dir ${EXP_DIR} --train-path ${TRAIN_PATH} --validation-path ${VALIDATION_PATH} --acceleration_factor ${ACC_FACTOR} --dataset_type ${DATASET_TYPE} --usmask_path ${USMASK_PATH} --teacher_checkpoint ${TEACHER_CHECKPOINT} --student_checkpoint ${STUDENT_CHECKPOINT}
python train_kd.py --batch-size ${BATCH_SIZE} --num-epochs ${NUM_EPOCHS} --device ${DEVICE} --exp-dir ${EXP_DIR} --train-path ${TRAIN_PATH} --validation-path ${VALIDATION_PATH} --acceleration_factor ${ACC_FACTOR} --dataset_type ${DATASET_TYPE} --usmask_path ${USMASK_PATH} --teacher_checkpoint ${TEACHER_CHECKPOINT} --student_checkpoint ${STUDENT_CHECKPOINT}
```

# Valid

To validate any model: `sh valid.sh`
```bash
BASE_PATH=''
MODEL='attention_imitation'
DATASET_TYPE='mrbrain'##cardiac,knee
MODEL_TYPE='teacher' ##student,kd

ACC_FACTOR='4x'##5x,8x
BATCH_SIZE=1
DEVICE='cuda:0'

DATA_PATH=${BASE_PATH}'/datasets/'${DATASET_TYPE}'/validation/acc_'${ACC_FACTOR}
CHECKPOINT=${BASE_PATH}'/experiments/'${DATASET_TYPE}'/acc_'${ACC_FACTOR}'/'${MODEL}'_'${MODEL_TYPE}'/best_model.pt'
OUT_DIR=${BASE_PATH}'/experiments/'${DATASET_TYPE}'/acc_'${ACC_FACTOR}'/'${MODEL}'_'${MODEL_TYPE}'/results'

echo python valid.py --checkpoint ${CHECKPOINT} --out-dir ${OUT_DIR} --batch-size ${BATCH_SIZE} --device ${DEVICE} --data-path ${DATA_PATH} --acceleration_factor ${ACC_FACTOR} --dataset_type ${DATASET_TYPE} --model_type ${MODEL_TYPE}
python valid.py --checkpoint ${CHECKPOINT} --out-dir ${OUT_DIR} --batch-size ${BATCH_SIZE} --device ${DEVICE} --data-path ${DATA_PATH} --acceleration_factor ${ACC_FACTOR} --dataset_type ${DATASET_TYPE} --model_type ${MODEL_TYPE}
```

# Evaluate

To evaluate on the reconstructions generated from the validation set: `sh evaluate.sh`
```bash
BASE_PATH=''
MODEL='attention_imitation'
DATASET_TYPE='mrbrain'##cardiac,knee
MODEL_TYPE='teacher'##student,kd

ACC_FACTOR='4x'##5x,8x

TARGET_PATH=${BASE_PATH}'/datasets/'${DATASET_TYPE}'/validation/acc_'${ACC_FACTOR}
PREDICTIONS_PATH=${BASE_PATH}'/experiments/'${DATASET_TYPE}'/acc_'${ACC_FACTOR}'/'${MODEL}'_'${MODEL_TYPE}'/results'
REPORT_PATH=${BASE_PATH}'/experiments/'${DATASET_TYPE}'/acc_'${ACC_FACTOR}'/'${MODEL}'_'${MODEL_TYPE}'/'

echo python evaluate.py --target-path ${TARGET_PATH} --predictions-path ${PREDICTIONS_PATH} --report-path ${REPORT_PATH} 
python evaluate.py --target-path ${TARGET_PATH} --predictions-path ${PREDICTIONS_PATH} --report-path ${REPORT_PATH} 
```

# Report 

To print out the evaluation metrics on the reconstructions generated from the validation set: `sh evaluate.sh`
```bash
BASE_PATH=''
MODEL='attention_imitation'
DATASET_TYPE='mrbrain'##cardiac,knee

MODEL_TYPE='kd'##teacher,student
ACC_FACTOR='4x##5x,8x
'

echo ${MODEL}
REPORT_PATH=${BASE_PATH}'/experiments/'${DATASET_TYPE}'/acc_'${ACC_FACTOR}'/'${MODEL}'_'${MODEL_TYPE}'/report.txt'
echo ${ACC_FACTOR}
cat ${REPORT_PATH}
echo "\n"
```