# Our training model
* date: 2021/12/13~15
* training process: 50 epoches / 9 hours
* total time: 34.5 hour

* training param: 
I forget to add --num_epochs=200, so i manually stopped it.
```console
CUDA_VISIBLE_DEVICES=0 python3 main.py --cub_dir ../../../../../data/dd/CUB_200_2011/ --name=bird_net --num_lbs 32 --symmetric_texture=False --nz_feat 256 --cam_loss_wt 2. --mask_loss_wt 2. --symmetric=False --print_freq 100 --display_freq 100 --boundaries_reg_wt 1. --bdt_reg_wt 0.1 --edt_reg_wt 0.1  --tex_size 6 --save_epoch_freq 10 --kp_loss_wt 50. --tex_loss_wt 1. --cub_dir ../../../../../data/dd/CUB_200_2011/
```

* evaluating param: 
modify --num_train_epoch
```console
CUDA_VISIBLE_DEVICES=0 python3 evaluate.py --split test --name bird_net --num_train_epoch 200 --num_lbs 32 --nz_feat 256  --symmetric_texture=False --symmetric=False --cub_dir ../../../../../data/dd/CUB_200_2011/  --batch_size 12
```

* bird_our_32: mean iou 0.68, pck.1 0.904, pck.15 0.962
* bird_net_32: mean iou 0.688, pck.1 0.897, pck.15 0.96
* bird_net_64: mean iou 0.711, pck.1 0.912, pck.15 0.965