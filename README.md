# Mask_Detection_Project
Classify images upon the presence of people wearing or not a face mask.

## First approach
At the beginning we tried to follow a "creative" approach, dividing the problem into three
subparts. Firstly, we searched for a premade network (MTCNN) that could extract faces from
pictures. Secondly, we trained a network to categorize the chunks given by the first one into
masked and non-masked faces. Lastly, we counted the number of predictions for each of the two
classes of the second network. Therefore, the final prediction on a complete picture was made by
applying this chain of operations. To train our net, we used the images from class 0 (no mask) and
1 (all mask), in order to assign a sure label to each patch automatically.



++
