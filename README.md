# Flower-Classification-with-TPUs
A kaggle playground competition, training and testing using TPU for a limited time (3h).
The author finished sixth in the competition, which was mainly due to the fact that I chose a result more in line with the rules of the competition (without using additional data sets) in the submission of the final answer, instead of choosing my highest score. But these are not important, the most important is that I learn and gain in the game, and hope to summarize these knowledge.

Solution tips and tricks

Data. 
I used external datasets prepared by @hengck23 and adapted by @kirillblinov . My experiment showed that openimage decrease accuracy, so I removed them. I used pictures with 331 sizes. 

Prepocessing. 
I tried autoaugment, randaugment (the basis was official tensorflow implementation for tf 1.X and then adapted code for tf 2.X with TPU) and training without any augmentations. My experiments showed that with big datasets it is better to use sample augmentations（like random_flip_left_right）.

Model selection. 
I tried all major efficientnet architectures. The best for me were b7 and b6 architecture with noisy-student pre-trained weights (great thanks to @pavel92) . The classical resnets (50 and 101) as well as Densenet-101 were not as well as efficientnet.
Model training. As it was noted by many participants, training without validation provide higher score on public leaderboard and my final solution included the models that were trained without validation part. I used exponential decay LR scheduler with warmup.

Ensembling. 
As i decided to use aggressive training strategy (no augmentations, no cross-validation), the ensembling was essential to avoid painful falling on private leaderboard. I trained best architectures (densenet, b7) with different picture sizes, try to combine effnet and seresnext models. The winner blend is two models B7-512 and Densenet-512. This solution also provides my well score on public LB.

Datasets.
Thanks to TPU and relatively small dataset, I could test a lot of hypothesis and considerably reduce my parameters grid. Then I added external datasets, but keep the same validation part. So after some experiments I could confirm that my validation score improved with external data and public score improved as well. At this step I defined best models (best combo of grid params)



Finally, I would like to make a statement that although I have achieved the sixth result (the best result is fourth), it is not in accordance with the rules of the competition. It USES additional data sets. However, the organizers of the competition made a statement at the end, which gave me no more time to change the model. I also tried to test without using additional data sets, and the results were around number 20. Now, ranking has become no longer important. Thank you for giving me a chance to practice in this competition, so that I can learn and use TPU.
