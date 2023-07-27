# Data-Model-for-Optimizing-item-allocation
![](https://foodindustryexecutive.com/wp-content/uploads/2017/08/bigstock-Modern-Supermarket-View-4236952-768x512.jpg)
A Data Model for Optimization of the Arrangement and Layout of products in Supermarkets

### The Goal is: 
making a research on how efficient is the allocation of items in shelves and also the layout of sections in a hypothetical supermarket and providing solutions to optimize it

### Steps of calculations:
I made it very simple to show the basics of the process, and for sure, it can be scalable to more complicated data structure
- I redrew the map of a real supermarket
![](https://github.com/samanshahin/Data-Model-for-Optimizing-item-allocation/blob/main/1.jpg)
![](https://github.com/samanshahin/Data-Model-for-Optimizing-item-allocation/blob/main/2.jpg)
- I provided a database with fake data (sales.csv) with 397884 rows (I just used 3 columns: sale_id, items, total)
- I provided a list of 100 grocery items and assigned a section_id to it (allocation.csv with 2 columns: item, cat_loc) 
- I randomly assigned grocery items to the sales database, it worked as a foreign key between sales and allocation data
```
for i in range(len(item_list)-1):
    r = random.randint(0, 99)
    new_list.append(items_list[r])
df['items'] = new_list
```
- I provided a weighted graph to save the graph data, containing sections as source/target points and steps between each node as weights for edges (paths.csv with 3 columns: x, y, dist) 
- Then I made a query to group sales data by item and count them, then I sorted the result
```
x = df.groupby('items').count()
x = x.sort_values(by='total')
```
- Then I fetched "most-visited sections" data by relating the "most-sold (best-selling) items" to data in the graph
```
shelves = []
 for i in range(len(x)):
   item = x['items'][i]
   ii = locs.loc[locs['item_name'].isin([item])]
   ix = ii['cat_loc'].values[0]
   shelves.append(ix)
sh.groupby(sh[0]).value_counts()
```
- As I didn't have access to camera footages and real data of how customers navigate the main court, I assumed the ShortestPath as the most passed path for each purchase, it was calculated using [this repository](https://github.com/arjun-menon/Distributed-Graph-Algorithms). In instead, we could calculate the most passed paths from each gate point, by callculating the average steps passed from all purchases
- In case of a scenario that a customer wants to approach to all sections and we have to find the best path, I used a manipulated version of a [MST algorithm](https://en.wikipedia.org/wiki/Minimum_spanning_tree) in [this repository](https://github.com/arjun-menon/Distributed-Graph-Algorithms) to find the minimal spanning tree path that covers all nodes.

## Limitation:
- The project was in research scale, not being targeted for commercial use
- I didn't have access to real data, neither camera footages to trace positions and regions where clients have been present (using OpenCV library). So, I simulated the situations
- I used several graph mining algorithms (e.g. Shortest-Path, Minimum-Spanning-Tree (MST) and NetworkX library) developed in Python, with Pandas, Numpy and MatPlotLib libraries

## Results:
The result was remarkable. There were some flaws and there should be some changes in layout and arrangement of sections...
- From marketing point of view, for promoting other less-sold (worst-selling) products and engaging customers to purchase them, the distribution of sections were effective but not good enough to cover all aspects
- From customer satisfaction point of view, the aim was assumed to be to decrease clients' tiredness and confusion, and to lessen the traffic jam, then energy consumption in the main court room, hence, some suggestions could be proposed for re-arrangement or re-allocation of shelves, in addition to make more spaces for clients movement

## Conclusion:
- The data model is highly scalable for more complicated data structure
- The main effort was to cover most favorable view of points (sales, marketing, CRM, financial issues)
- In most supermarkets, it has been solved impirically, i.e. not using calculations and graph mining solutions.
- In a small survey that I had with my local network, I realized that due to the economic situations and business difficulties, business owners have a doubt about the efficiency and impact of using scientific solutions, especially data science in their working process and there's no interest in spending on R&D projects like this to optimize the work flow, even if it has an impact, it's not "highly" justifiable to change the established working criteria, as most of them said.
