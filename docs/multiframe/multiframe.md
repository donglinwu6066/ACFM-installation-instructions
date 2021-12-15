# Our training model
* date: 2021/12/15
* training process: 
* total time: 

## Resource monitoring for monocular
### Hardware: 
CPU: i7-8700K
* RAM: 17~18G

GPU 1: 1080 Ti
* temperature: 65~70 °C
* Volatile GPU-util: 90~100%
* RAM: ~8.5G

GPU 2: 1080 Ti
* temperature: 55~60 °C
* Volatile GPU-util: 90~100%
* RAM: ~6G

## Training param: 
```console
CUDA_VISIBLE_DEVICES=0,1 python -B main.py --name=horse_our_nokp --category horse --display_port 8097 --batch_size=8 --learning_rate 1e-4 --num_lbs 16 --nz_feat 256 --symmetric_texture=False --num_guesses 6 --use_gtpose=False --symmetric=False --mesh_dir meshes/horse.obj  --root_dir ../../../../../data/dd/TigDog_new_wnrsfm/ --tmp_dir tmp_horsenokp/ --kp_loss_wt 0. --of_loss_wt 0.1 --mask_loss_wt 2. --boundaries_reg_wt 1. --bdt_reg_wt 0.1 --edt_reg_wt 0.5  --deform_reg_wt 0.0001  --rigid_wt 10. --display_freq 100  --drop_hypothesis  --print_freq 100  --cam_loss_wt 2.  --scale_lr_decay 0.1  --root_dir_yt ../../../../../data/dd/TigDog_new_wnrsfm/ --expand_ytvis=True  --save_epoch_freq 10 --num_reps 20 --tex_num_reps 20 --tex_loss_wt 1. --az_el_cam True --scale_bias=0.9 --az_euler_range 360 --num_kps 19 --triangle_reg_wt 0.01  --optimize_deform --warmup --texture_warmup --scale_mesh=True --num_epochs 200
```

## Evaluating param: 

## Error notes
if the system show any unknown errors, you can try to download dataset again, e.g.:
```console
q_mult_0 = qa_0 * qb_0 - qa_1 * qb_1 - qa_2 * qb_2 - qa_3 * qb_3
RuntimeError: The size of tensor a (192) must match the size of tensor b (24) at non-singleton dimension 0
```

## Results: