# SURE_Scheme2022
It is a code that aims to detect anti-hale sunspots by comparing the hemispheres of the Sun using image-processing in Python. The code uses a library called sunpy (https://sunpy.org/).

Firstly, The code finds the polarity regions as Hale's Law of Polarity is not applicable to singular sunspots. Then, it checks if the polarity region has a region with magnetic field strength of 1000 Gauss. After this process, it reflects those regions to the other hemisphere and compares if the polarities are reversed in the other hemisphere. If it is not reversed, it counts them as an Anti-Hale region. 


This part of the code allows to import any packages necessary for the rest of the code
```python
import ...
```

This allows to fetch and download the HMI data at a specific time from a specific instrument. 'print(result)' allows to check the information stored in the HMI data.
```
result = Fido.search(a.Time('2014/04/09 06:00:00', '2014/04/09 06:10:00'), 
                     a.Instrument.hmi, a.Physobs.los_magnetic_field)
#print(result)
downloaded_file = Fido.fetch(result)
```

You can change the coordinates of the cut to focus on different areas as the latitude of the sunspots change during the cycle.
```
bottom_left = SkyCoord(-700 * u.arcsec, -500 * u.arcsec, frame=hmi_rotated.coordinate_frame)
top_right = SkyCoord(700 * u.arcsec, 500 * u.arcsec, frame=hmi_rotated.coordinate_frame)
hmi_smap = hmi_rotated.submap(bottom_left, top_right=top_right)
fig = plt.figure()
ax = fig.add_subplot(projection=hmi_rotated)
hmi_smap.plot(axes=ax, clip_interval=(1, 99.99)*u.percent, autoalign=True)
bounds = ax.axis()
ax.axis(bounds)
params = {"text.color" : "white"}
plt.rcParams.update(params)
ax.set_xlabel("Helioprojective Longitude (Solar-X)",color = "white") 
ax.set_ylabel("Helioprojective Latitutde (Solar-Y)", color = "white")
ax.tick_params(axis='x', colors='white')
ax.tick_params(axis='y', colors='white')
plt.show()
```

These lines allow  to find the general shape of the bipolar regions by choosing the areas with the the minimum magnetic stregth of 100 or -100 Gauss and contour them. This allows to find the boundaries of the polarity regions. Also, the sign of the magnetic field strength indicates the direction of the magnetic field region.
```
levels = [-100, 100] * u.Gauss
...
hmi_contour_neg = hmi_smap.contour(-100* u.Gauss)
hmi_contour_pos = hmi_smap.contour(100*u.Gauss)
```

The Skycoord is converted to strings and then coordinates to reflect the region that the code has found to the other hemisphere. The code checks the regions that have been found if they have three points or not because if they are less than 3, they cannot be counted a polygon/region. 

Some buffer regions are added by using ```Polygon().buffer ```. The areas, that are close to each other, are combined together using ```unary_union()``` to expand the area of the regions because once they are reflected there may not a region at the exact location and it can compare more areas with this way. Then, magnetic regions with different directions that are close to each have been combined together to find the polarity/bipolar regions using ```unary_union()```. 

Once the polarity regions have been found in both hemispheres, a contour with 1000/-1000 Gauss is added to find if the regions are containing any sunspots with these lines
```
hmi_contour_neg_ss = hmi_smap.contour(-1000* u.Gauss)
hmi_contour_pos_ss = hmi_smap.contour(1000*u.Gauss)
```
Then, the polarity regions without any sunspots are removed and the code compares two hemispheres if the polarities are reversed in the other hemisphere.

``` common_elements_south_pos, common_elements_north_pos, common_elements_south_neg,  common_elements_north_neg``` find the indices of the regions that are Anti-Hale regions.


```ch_boundary = SkyCoord(
    list(zip(*pos_s_combined[290].exterior.coords.xy))*u.arcsec,
    obstime=hmi_rotated.date, observer="earth",
    frame=frames.Helioprojective)
``` 
This part of the code allows to visualise the areas that the code has detected as an Anti-Hale region. It allows to check if the code is detecting the right areas or not. You can change ```pos_s_combined[] to pos_n_combined[], neg_s_combined[], neg_n_combined[]``` to check if the code is detecting the right areas and you can change the index number in the sqaured paranthesis in ```pos_s_combined[], pos_n_combined[], neg_s_combined[], neg_n_combined[]``` with the index numbers you get from the cells above which will allow you to check if the code is detecting an Anti-Hale region or not.


Another important note is that this a product of a 6-week summer research project, therefore the code may not be comprehensive enough to show all the solar physics necessary to detect the Anti-Hale regions and feel free to contact me via email if you find any parts confusing: melisabozac@gmail.com


![image](https://user-images.githubusercontent.com/34107944/201670007-c5576b12-5616-4bf8-baea-087ab68fef08.png)
![image](https://user-images.githubusercontent.com/34107944/201670077-abc6c865-0b12-4039-9c0e-0fa1ef80da95.png)
