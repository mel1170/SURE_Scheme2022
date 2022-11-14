# SURE_Scheme2022
It is a code that aims to detect anti-hale sunspots by comparing the hemispheres of the Sun using image-processing in Python. The code uses a library called sunpy (https://sunpy.org/).
Firstly, The code find the polarity regions as Hale's Law of Polarity is not applicable to singular sunspots. Then, it checks if the polarity regions has a region with magnetic field strength of 1000 Gauss. After this process, it reflects those regions to the other hemisphere and compares if the polarities are reversed in the other hemisphere. If it is not reversed, it counts them as an Anti-Hale region. 
Feel free to change the date to look at the HMI data you want to look in the second cell.
In the cell with explanation with "Coordinates of the Contoured Area", you can change pos_s_combined[] to pos_n_combined[], neg_s_combined[], and neg_n_combined[] to check if the code is detecting the right areas.
Moreover, you can change the number in the sqaured paranthesis in pos_s_combined[], pos_n_combined[], neg_s_combined[], and neg_n_combined[] in that cell to the numbers you get from the cells above which will allow you to check if the code is detecting an Anti-Hale  region or not.
Another important note is that this a product of a 6-week summer research project, therefore the code may not be comprehensive enough to show all the solar physics necessary to detect the Anti-Hale regions and feel free to contact me via email if you find any parts confusing: melisabozac@gmail.com
![image](https://user-images.githubusercontent.com/34107944/201668259-7737bd6c-b8cf-4de9-920a-540f9491f43c.png)
![image](https://user-images.githubusercontent.com/34107944/201668302-03fcf4e9-5562-49ff-95f3-0c4b339be92b.png)
