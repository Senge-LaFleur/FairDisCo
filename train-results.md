FAIRDISCO
....... processing ........

hyper-parameters alpha: 1.0  beta: 0.8
Epoch 0/2
----------
train
Accuracy: 8517.0/12809
train Loss: 7.0713 Acc: 0.6649 Balanced-Acc: 0.6677
val
Accuracy: 2051.0/3203
val Loss: 6.7348 Acc: 0.6403 Balanced-Acc: 0.6603
Epoch 1/2
----------
train
Accuracy: 10192.0/12809
train Loss: 6.2829 Acc: 0.7957 Balanced-Acc: 0.8009
val
Accuracy: 2187.0/3203
val Loss: 6.5510 Acc: 0.6828 Balanced-Acc: 0.6778
New leading accuracy: 0.682797372341156
Epoch 2/2
----------
train
Accuracy: 10951.0/12809
train Loss: 5.6967 Acc: 0.8549 Balanced-Acc: 0.8585
val
Accuracy: 2321.0/3203
val Loss: 6.6546 Acc: 0.7246 Balanced-Acc: 0.7159
New leading accuracy: 0.7246331572532654
Training complete in 64m 53s
Best val Acc: 0.724633
Best model epoch: 2
Training Complete
gold

 Accuracy: 0.7246331564158601  Balanced Accuracy: 0.7158590300078483

done

FAIRDISCO EVALUATION
acc_avg array
[0.22906292]
acc per type
[[0.2173913  0.23293608 0.23401163]]
PQD
[0.92897651]
DPM
[0.83156509]
EOM
[0.79216807]
AUC
[-1.]

average accuracy mean: 0.22906292440018108, std: 0.0
accuracy per skin type mean and std
[0.2173913  0.23293608 0.23401163] [0. 0. 0.]
PQD mean: 0.9289765055360518, std: 0.0
DPM mean: 0.8315650879308714, std: 0.0
EOM mean: 0.7921680651829494, std: 0.0
AUC mean: -1.0, std: 0.0




FAIRDISCO + LMI
....... processing ........

hyper-parameters alpha: 1.0  beta: 0.8 lam_mi: 1.0
Epoch 0/2
----------
train
Accuracy: 8515.0/12809
train Loss: 7.2185 Acc: 0.6648 Balanced-Acc: 0.6710
val
Accuracy: 2077.0/3203
val Loss: 6.5904 Acc: 0.6485 Balanced-Acc: 0.6597
Epoch 1/2
----------
train
Accuracy: 10255.0/12809
train Loss: 6.3463 Acc: 0.8006 Balanced-Acc: 0.8059
val
Accuracy: 2418.0/3203
val Loss: 6.4248 Acc: 0.7549 Balanced-Acc: 0.6546
New leading accuracy: 0.7549172639846802
Epoch 2/2
----------
train
Accuracy: 11018.0/12809
train Loss: 5.7716 Acc: 0.8602 Balanced-Acc: 0.8642
val
Accuracy: 2398.0/3203
val Loss: 6.5092 Acc: 0.7487 Balanced-Acc: 0.7015
Training complete in 121m 40s
Best val Acc: 0.754917
Best model epoch: 1
Training Complete
gold

 Accuracy: 0.7549172650640025  Balanced Accuracy: 0.6546285184932823

done


FAIRDISCO + LMI EVALUATION
acc_avg array
[0.19103667]
acc per type
[[0.19063545 0.18309859 0.20203488]]
PQD
[0.90627217]
DPM
[0.85916304]
EOM
[0.76315647]
AUC
[-1.]


average accuracy mean: 0.19103666817564507, std: 0.0
accuracy per skin type mean and std
[0.19063545 0.18309859 0.20203488] [0. 0. 0.]
PQD mean: 0.9062721653662985, std: 0.0
DPM mean: 0.8591630447465578, std: 0.0
EOM mean: 0.7631564652430357, std: 0.0
AUC mean: -1.0, std: 0.0