# Preseed
Auto Debian installation with Preseed


# Disk partition format

When a partition is introduced, 3 numbers will be considered for it. minimum, priority and maximum!
From the left,minimum, priority and maximum number.

Priority is used to prioritize when we have allocated minimums to our partitions and still have space that is not allocated to any partition. In fact, we consider the weight for each partition with priority to determine its priority.

To calculate the weight of each partition, we consider two things.
If the priority number of the partition is less than or equal to the minimum number, the weight of this partition is considered 0.
Otherwise, the absolute value of the difference between priority and minimum weight will be related to our partition.

If I consider the above example:


weight   minimum   priority   maximum   name
0        8192      8241       16384     swap
2        16384     16386      -1        root
0        8192      8241       16384     var
