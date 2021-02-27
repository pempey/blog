![A page of paper with hand written text and cut out images placed on top](../assets/ff8_hero.jpg)

# Fundamental Friday #8

## The Question

This week I tasked my self with making a pie chart that answers the following question:

> How did the value of B compare to all other categories last month

## The Design

This week I took a different approach to the design. I went with a more exploratory method of design as one might when they do not know what the data was or if you were given a large data set.  As I decided to go with Python for my tool this week I took a step by step approach and kept the full process in a jupyter notebook to preserve my steps as you might need to do for a repeated process.  This kind of process helps when you may have to automate the visualization creation and is similar to the approach a Data Scientist may take  during the initial discovery period of a new project.

But will does leave the description of the formal design this week very short, so on to the process.

## The Process

After loading in the data I took a loot at the first few records in the data set. 

```python
practice_data.head()
```

| ID | Date | Category | Value |
| -- | ---- | -------- | ----- |
| 0|1|2/21/2019|	B|	79|
| 1|2|2/22/2019|	E|	74|
| 2|3|2/23/2019|	B|	86|
| 3|4|2/24/2019|	B|	82|
| 4|5|2/25/2019|	E|	65|

--- 


Seeing that I had more data than I needed I filtered the data and grouped it to be just the single value per category that I would need for a pie chart.

```python
filterd_data = practice_data[(practice_data['Date'] >= '1/1/2021') & (practice_data['Date'] <= '1/31/2021')]
grouped_data = filterd_data.groupby(by="Category")["Category","Value"].sum()
grouped_data
```
	
|Category|Value|
|--------|-----|
|B	|1127|
|C	|75|
|D	|132|
|E	|661|

---

With that I made a quick and simple pie chart to see what it would look like so I could figure out what other changes to the data I might need and to start getting and idea for the style I wanted.

```python
grouped_data["Value"].plot.pie(autopct="%.1f%%")
```
![A page of paper with hand written text and cut out images placed on top](../assets/ff8_process_1.jpg)

Given the question, I decided I group all of the not B categories together to that the direct comparison will be easter for the reader.

```python
renamed_data =  filterd_data
renamed_data.loc[renamed_data["Category"] != 'B', 'Category'] = 'Others'

regrouped_data = renamed_data.groupby(by="Category")["Category","Value"].sum()
regrouped_data
```
	
|Category |Value|
|--------|----|	
|B	|1127|
|Others	|868|

---

Then I looked at the pie chart again and started to implement some styling.  I wanted there to be a line at the 12 o’clock position so I rotted the starting position 90 degrees, changing the starting line from a horizontal line to a vertical line.

```python 
regrouped_data["Value"].plot.pie(autopct="%.1f%%",startangle=90)
```
![A page of paper with hand written text and cut out images placed on top](../assets/ff8_process_2.jpg)

Now that I had the raw shape I was looking for the reset was down to styling.  I started with selecting the size of the final figure so that I would be easier to read.  Then I selected colors so that in the visual hierarchy the B category was higher than the Others category. 

I also wanted the category labels and the percent of the category on the inside of the wedge.  I was going to have enough space and having them together on the inside saves space and prevents the reader from having to move around the figure to connect the category with the numeric value.

Other elements included removing axis labels, (pie charts don’t really have axis anyway right?) adding a title, label position and justification, and selecting font sizes and colors.

```python
figure = plt.figure(figsize=(8,8))

colors = ['tab:blue','tab:gray']

def chart_lables(data):
    category = data.index
    total = int(data.sum())
    pct = [x*100 / total for x in data.Value]
    return [category +'\n{:.1f}%'.format(pct)  for pct,category in zip(pct,category)]

labels = chart_lables(regrouped_data)

regrouped_data["Value"].plot.pie(
                                    startangle=90,
                                    textprops=dict(
                                        color='w',
                                        fontsize= 30,
                                        weight='bold',
                                        ha='center'
                                    ),
                                    labeldistance=0.5,
                                    colors=colors,
                                    labels=labels
                                ) 
figure.suptitle('The Values of B and Others for \nJanuary 2021', fontsize = 30,wrap=True)
plt.ylabel('')
```

And that resulted in the final chart.

![A page of paper with hand written text and cut out images placed on top](../assets/ff8_final.jpg)

## What I Learned

I learned a lot about python using this design style and I learned that the design process can be very discovery based.  I guess it is a lot like story writing where you can be an outliner or a discovery writer.  I liked the process and will be doing more of it in the future.

Thanks for reading and I hope that you learned something with me along the way.
