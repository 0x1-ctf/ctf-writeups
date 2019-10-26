# Lying Out (forensics, 250pts)

> Some odd [traffic](./assets/traffic.png) has been detected on the network, can you identify it? More info here.
> Connect with nc 2018shell.picoctf.com 39410 to help us answer some questions.

The image shows the number of IPs connecting to a service over time.

Connect to the server and answer the question to get the flag.  You're looking for any values that are above the
vertical bars at the specified time.

```
$ nc 2018shell.picoctf.com 39410
Which of these logs have significantly higher traffic than is usual for their time of day? You can see usual traffic on
the attached plot. There may be multiple logs with higher than usual traffic, so answer all of them! Give your answer
as a list of `log_ID` values separated by spaces. For example, if you want to answer that logs 2 and 7 are the ones with
higher than usual traffic, type 2 7.
    log_ID      time  num_IPs
0        0  00:00:00    10266
1        1  01:00:00    11590
2        2  01:45:00    11620
3        3  03:00:00     9822
4        4  07:00:00    10167
5        5  10:15:00    10403
6        6  10:30:00     9779
7        7  15:00:00     9664
8        8  15:45:00    10236
9        9  16:45:00    10523
10      10  17:45:00    15760
11      11  18:30:00    13389
12      12  20:15:00     9889
13      13  21:00:00    11637
1 2 10 13
Correct!

Great job. You've earned the flag: picoCTF{w4y_0ut_940df760}
```
