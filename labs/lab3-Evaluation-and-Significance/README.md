# Lab 3: Evaluation and Significance

Today's lab is about evaluating learning algorithm. I've provided the
true labels from last year's P1 "last part" assignment (note: it was a
different task), and the solutions produced by the top three
teams.

You can start by loading ``eval.py`` into a python shell. This will
load in the true labels to the variable ``Y`` and the top three
solutions into ``allP[0]``, ``[1]`` and ``[2]``, respectively (0 is
the "winner").

A) Compute the accuracies of these three solutions. This should be a
one-linear in python using numpy. The labels are 0 or 1, and the
predicted values are all between 0 and 1. You should treat anything
over 0.5 as "1" and <=0.5 as "0". Hint: look at np.average or
np.mean. If you cannot do it in one line (think about it first!) ask
me.

>>> np.count_nonzero(np.greater(allP[0], .5) == Y)/ float(Y.size)
0.7502952755905512
>>> np.count_nonzero(np.greater(allP[1], .5) == Y)/ float(Y.size)
0.7382874015748031
>>> np.count_nonzero(np.greater(allP[2], .5) == Y)/ float(Y.size)
0.7375
>>>
///////////////////////////////// alternate solution //////////////////
>>> np.mean(np.greater(allP[0], .5) == Y)
0.7502952755905512
>>> np.mean(np.greater(allP[1], .5) == Y)
0.7382874015748031
>>> np.mean(np.greater(allP[2], .5) == Y)
0.73750000000000004


You can draw precision/recall curves for this data using:

```python
>>> makeManyCurves(Y,allP)
```

In this, the X-axis is precision and the Y-axis is recall. This is
obtained by varying the threshold away from 0.5.

B) Does a high threshold lead to high precision and low recall or high
recall and low precision? What about a low threshold?

1 - 1 = TP
1 - 0 = FN
0 - 1 = FP
0 - 0 = TN  

Precision = TP / TP + FP
Recall = TP / TP + FN


High Threshold
a high threshold lead to high precision and low recall because the number of False Positives will decrease and False Negatives(number of 1s cl) will increase.

Low Threshold
A low threshold will lead to a lot of False positives so my precision will drop. 
Recall goes higher as the number of false negatives will decrease.

Sometimes you care about high precision systems. For instance, I might
say "if I require a precision of at least 0.75, how high of a recall
can I get? This means that I want a system that, when it says "yes"
it's right 75% of the time... what percentage of the true "yes"
answers can I find.

C) This measure is called "recall at P=0.75" -- what is it
(approximately) for the three curves?

for data 0 is .7830
for data 1 is .7442
for data 2 is .7471

I've given you a partial implementation of the ttest, but you'll need
to fill in some of the details. Once you've done that, you can run
something like:

```python
>>> ttest(Y, allP[1], allP[0])
(4.1490355592633517, 'significant at 99.5% level')
```

This says that the first place team is better than the second place
team with 99.5% confidence.

```python
>>> ttest(Y, allP[2], allP[1])
(0.20950326198721958, 'not significant at 90% level')
```

This says that the second place team is not significantly better than
the third place team, even at 90%.

Now, if you have less data, you are often less likely to be able to
tease apart significant differences. We can force the ttest to run on
a subset of the data:

```python
>>> ttest(Y, allP[1], allP[0], restrictTo=500)
(0.88445909887276331, 'not significant at 90% level')

>>> ttest(Y, allP[1], allP[0], restrictTo=750)
(2.0814897135353521, 'significant at 97.5% level')
```

You can plot the t-statistic as a function of the amount of data
using:

```python
>>> plt.plot([ttest(Y,allP[1],allP[0], i)[0] for i in range(10160)])
>>> plt.show()
```

(There will be a couple warnings that you can ignore.)

D) Based on this, how much data do you need to get 95% significance?
99.5% significance?  Around 658 samples, Around above 738 to 762, then back 

If I want to find all the data samples where I could get 95% significance, I ran this and got all the number of samples i need to get 95% under 5000. Above 5000
is always different t values.

for i in range(5000):
   tt.append(ttest(Y,allP[1],allP[0],i))

>>> for i, (x, y) in enumerate(tt):
...    if (x < 1.95 and x >= 1.64):
...        a.append(i)


[656, 766, 767, 768, 769, 770, 771, 772, 773, 774, 775, 776, 841, 842, 843, 844, 845, 846, 847, 848, 849, 850, 851, 852, 853, 854, 855, 856, 857, 858, 859, 860, 861, 862, 863, 864, 865, 866, 867, 868, 869, 870, 871, 872, 873, 874, 875, 876, 877, 878, 879, 880, 881, 882, 883, 884, 885, 886, 887, 888, 889, 890, 891, 892, 893, 955, 970, 971, 972, 973, 974, 975, 976, 977, 978, 979, 980, 981, 982, 983, 984, 985, 986, 987, 988, 989, 990, 991, 992, 993, 994, 995, 996, 997, 998, 999, 1000, 1001, 1002, 1003, 1004, 1005, 1006, 1007, 1008, 1009, 1010, 1011, 1012, 1013, 1014, 1015, 1016, 1017, 1018, 1019, 1020, 1021, 1022, 1023, 1024, 1025, 1026, 1027, 1028, 1029, 1030, 1031, 1032, 1033, 1034, 1035, 1036, 1037, 1038, 1039, 1040, 1041, 1042, 1043, 1044, 1045, 1046, 1047, 1048, 1049, 1050, 1051, 1052, 1053, 1054, 1055, 2410, 2411, 2412, 2413, 2414, 2415, 2416, 2417, 2418, 2419, 2420, 2421, 2422, 2423, 2424, 2772, 2773, 2774, 2775, 2776, 2777, 2778, 3063, 3064, 3082, 3083, 3084, 3085, 3086, 3087, 3088, 3089, 3090, 3091, 3092, 3093, 3094, 3095, 3096, 3097, 3098, 3099, 3100, 3101, 3102, 3103, 3104, 3105, 3106, 3107, 3108, 3109, 3110, 3111, 3112, 3113, 3114, 3115, 3116, 3117, 3118, 3119, 3120, 3121, 3122, 3123, 3124, 3125, 3126, 3127, 3128, 3129, 3130, 3131, 3132, 3133, 3134, 3135, 3136, 3137, 3138, 3139, 3140, 3141, 3142, 3143, 3144, 3145, 3146, 3147, 3148, 3149, 3150, 3151, 3152, 3153, 3154, 3155, 3156, 3157, 3158, 3159, 3160, 3161, 3162, 3163, 3164, 3165, 3166, 3167, 3168, 3169, 3173, 3182, 3183, 3184, 3185, 3186, 3187, 3188, 3189, 3190, 3191, 3192, 3193, 3194, 3195, 3196, 3197, 3198, 3199, 3200, 3201, 3202, 3203, 3204, 3205, 3206, 3207, 3208, 3209, 3210, 3211, 3212, 3213, 3214, 3215, 3216, 3217, 3218, 3219, 3220, 3221, 3222, 3223, 3224, 3235, 3236, 3237, 3238, 3239, 3240, 3241, 3242, 3243, 3244, 3245, 3246, 3247, 3248, 3249, 3250, 3251, 3252, 3253, 3254, 3255, 3256, 3257, 3258, 3259, 3260, 3261, 3262, 3263, 3264, 3265, 3266, 3267, 3268, 3269, 3270, 3271, 3272, 3273, 3274, 3275, 3276, 3277, 3278, 3279, 3280, 3281, 3282, 3283, 3284, 3285, 3286, 3287, 3288, 3289, 3290, 3291, 3292, 3293, 3294, 3295, 3296, 3297, 3298, 3299, 3300, 3301, 3302, 3303, 3304, 3305, 3306, 3307, 3308, 3309, 3310, 3311, 4004, 4005, 4006, 4016, 4017, 4018, 4019, 4020, 4021, 4022, 4023, 4024, 4025, 4026, 4027, 4028, 4029, 4030, 4031, 4032, 4033, 4034, 4035, 4036, 4037, 4038, 4039, 4040, 4041, 4042, 4043, 4044, 4045, 4046, 4047, 4048, 4049, 4050, 4051, 4052, 4053, 4054, 4055, 4056, 4057, 4058, 4059, 4060, 4061, 4062, 4063, 4064, 4065, 4066, 4067, 4068, 4069, 4070, 4071, 4072, 4073, 4074, 4075, 4076, 4077, 4078, 4079, 4080, 4081, 4082, 4083, 4084, 4085, 4086, 4087, 4088, 4089, 4090, 4091, 4092, 4093, 4094, 4095, 4096, 4097, 4098, 4099, 4100, 4101, 4102, 4103, 4104, 4105, 4106, 4107, 4108, 4109, 4110, 4111, 4112, 4113, 4114, 4115, 4116, 4117, 4118, 4119, 4120, 4121, 4122, 4123, 4124, 4125, 4126, 4127, 4128, 4129, 4130, 4131, 4132, 4133, 4134, 4135, 4136, 4137, 4138, 4139, 4140, 4141, 4142, 4143, 4144, 4145, 4146, 4147, 4148, 4149, 4150, 4151, 4152, 4153, 4154, 4155, 4156, 4157, 4158, 4159, 4160, 4161, 4162, 4163, 4164, 4165, 4166, 4167, 4168, 4169, 4170, 4171, 4172, 4173, 4174, 4175, 4176, 4177, 4178, 4179, 4180, 4181, 4182, 4183, 4184, 4185, 4186, 4187, 4188, 4189, 4190, 4191, 4192, 4193, 4194, 4195, 4196, 4197, 4198, 4199, 4200, 4201, 4202, 4203, 4204, 4205, 4206, 4207, 4208, 4209, 4210, 4211, 4212, 4213, 4214, 4215, 4216, 4217, 4218, 4219, 4220, 4221, 4222, 4223, 4224, 4225, 4226, 4227, 4228, 4229, 4230, 4231, 4232, 4233, 4234, 4235, 4236, 4237, 4238, 4239, 4240, 4241, 4242, 4243, 4244, 4245, 4246, 4247, 4248, 4249, 4250, 4251, 4252, 4253, 4254, 4255, 4256, 4257, 4258, 4259, 4260, 4261, 4262, 4263, 4264, 4265, 4266, 4267, 4268, 4269, 4270, 4271, 4272, 4273, 4274, 4275, 4276, 4277, 4278, 4279, 4294, 4295, 4296, 4297, 4298, 4299, 4300, 4301, 4302, 4303, 4304, 4601, 4602, 4603, 4604, 4605, 4639, 4640, 4641, 4642, 4643, 4644, 4645, 4646, 4647, 4657, 4658, 4659, 4660, 4661, 4662, 4663]


E) You'll note that this is not a monotonically increasing
function. Why does this happen? What does it mean from the perspective
of testing significance?

Basically there is not a reason for this graph to be monotonically increasing.
The ttest is proportional to some properties and inversely proportional to others. I can compare both algorithms, and as I get more data I should be getting a bigger significance as there is a lot more data to help both algorithms. Using these tool to decide when to reject the null hypothesis increases our chance of classifying correctly between both algorithms. However, there is not a direct variable behind that will account for noise, irrelevant feature, etc in different subsets. 

Significance levels still help us quantify and control the error in the hypothesis t test. However ttest is sensitive to anomalous values in the data. They increase the estimate of sample variance, thus decreasing the calculated t statistic and lowering the chance of rejecting the null hypothesis. 



