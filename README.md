# RecyclerView-Lesson7

This is the Sleep Tracker app with recycler view for lesson 7 of the Udacity: Developing Android Apps with Kotlin Course. 
- RecyclerView
- Displaying the data in list or grid view
- Learn about Adapter pattern

### Introduction:
Displaying a list of data is one of the most common UI tasks in Android. If you look at the apps on your phone, almost every single app has at least one list. To support all the use cases, from displaying list of simple data like TextView to complex data like cardView(Instagram), android comes with the RecyclerView widget. 

RecyclerView 
- Efficient in displaying extremely large lists. 
- Display complex items 
- Highly customizable

RecyclerView Layout
- It supports layout list or grid
- It supports scroll horizontal or vertical 
- Supports custom layouts 

It uses an adapter pattern, to minimize the work it does when drawing  a list. By default, it will only do work to process or draw items that are currently visible on the screen.  That means, if your list has 1000 elements but only 10 are visible, it will only do enough for it to draw the first 10 items on screen until the user scrolls. When the user scrolls, RecyclerView will figure out what new items should be on screen and to do just enough work to display those. What’s really nice is, we don’t have to do anything to get this behavior it comes by default in recyclerView. 
Another way that RecyclerView is efficient is by reusing or recycling as much as it can. 

We will be adding RecyclerView to the SleepTracker App. And show the data in a list and grid format. And we will also learn about the adapter pattern used by RecyclerView. 

——

### Adapter:
- Converts one interface to work with another.  - If you ever traveled between countries that use different electric sockets, you already know how you could plug your devices into outlets by using an adapter. The adapter lets you convert one type of plug to another, which is really one interface to another and make it work nicely.

#### Adapter Pattern
The adapter pattern in software engineering helps an object to work together nicely with another API just like how a power adapter lets your laptop charge when your are traveling. The adapter pattern is one of the first design patterns. Design patterns are simply common ways to design software that worked for a lot of developers. They’re similar to architectural patterns but they are more about solving common coding problems. 

#### How does this adapter pattern fit in the RecyclerView?
![Adapter](https://user-images.githubusercontent.com/43662326/152430725-3a18a19f-2893-4677-b436-910f15fee0d0.png)

In the case of recycler view we have our apps data which we want to display as a list. We don’t want to change the way our app stores or processes data just to put it onto the screen. So we built an adapter which adapts our data into something that can be used by RecyclerView. 

In our app, we’ll be building an adapter that adapt list for use by RecyclerView. But the adapter is a general pattern we can apply it to anything. No matter how we store our data, if it can be presented in a way that recycler view can use, you can build an adapter to convert it. 

#### Adapter Interfaces
RecyclerView adapters must provide a few methods for RecyclerView to understand how to display the data on screen. 

Things needed for RecyclerView Adapter:
- How many items - First, in order to know how far to scroll, the adapter needs to tell the recycler view how many items are available. 
- How to draw an item - than you provide a way for the RecyclerView to draw a specific item. 
- Create a new view - finally, you must provide RecyclerView with a way to create a new view for an item. 
- ViewHolder - To implement recycling and support multiple types of views, RecyclerView doesn’t interact to views but instead ViewHolders. This is the last part of the adapter interface. 
    - ViewHolders do exactly what it sounds like they do, hold views. 
    - Store information for RecyclerView
    - RecyclerView’s main interface - ViewHolders know things like the last position the items have in the list, which is important when you are animating list changes. 

——

### Add a RecyclerView

1. Add RecyclerView to layout. Open fragment_sleep_tracker.xml and replace the entire ScrollView, including the enclosed TextView, with a RecyclerView:
```
<androidx.recyclerview.widget.RecyclerView
  android:id="@+id/sleep_list"
  android:layout_width="0dp"
  android:layout_height="0dp"
  app:layout_constraintBottom_toTopOf="@+id/clear_button"
  app:layout_constraintEnd_toEndOf="parent"
  app:layout_constraintStart_toStartOf="parent"
  app:layout_constraintTop_toBottomOf="@+id/stop_button"
  app:layoutManager="androidx.recyclerview.widget.LinearLayoutManager"/>
```
In SleepNightAdapter.kt
1. Create a file called SleepNightAdapter.kt
```class SleepNightAdapter: RecyclerView.Adapter<TextItemViewHolder>()```
2. Define a data source. Create variable data and assign it to listOf<SleepNight>. 
3. Override getItemCount() and have it return data.size. 
4. Override onBindViewHolder() -> The function should retrieve the item from the data list, and set holder.textView.text to item.sleepQuality.toString(). 
5. Override onCreateViewHolder(). -> NOTE The Adapter requires this method, but we won't fill it in here. We’ll cover it in a later exercise, but for now it just make sure it contains the line, TODO("not implemented"), so the code will compile. 
6. Verify that the code compiles and runs without errors.
7. Now to tell RecyclerView api how to create a new view holder. - the api for that is called onCreateViewHolder() In SleepNightAdapter, onCreateViewHolder(), inflate the text_item_view layout and return the ViewHolder.
    ```override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): TextItemViewHolder {
       val layoutInflater = LayoutInflater.from(parent.context)
       val view = layoutInflater
            .inflate(R.layout.text_item_view, parent, false) as TextView
       return TextItemViewHolder(view)
 }```
8. In SleepNightAdapter, add a custom setter to data that calls notifyDataSetChanged() and tell Kotlin to save the new value by setting field = value.
  ```
set(value) {
   field = value
   notifyDataSetChanged()
 }
```

9. In SleepTrackerFragment, create a new SleepNightAdapter, and use binding to associate it with the RecyclerView:
    ``` binding.sleepList.adapter = adapter ```
10. Create an observer on sleepTrackerViewModel.nights that sets the Adapter when there is new data.
    LiveData observers are sometimes passed null, so make sure you check for null. 
——

### Why ViewHolders are used by RecyclerView?

```class TextItemViewHolder(val textView: TextView): RecyclerView.ViewHolder(textView)```
RecyclerView relies upon the functionality above to correct the position the view as the list scrolls and do interesting things like animating views when items get edit or removed in the adapter. Because it has all of this information available in the ViewHolder, RecyclerView can even continue to animate the view while they are moving from scrolling. 
    
A ViewHolder tells a RecyclerView where and how an item should get drawn in the list. We can see “itemView” which is the reference that RecyclerView reviews when it needs to access the actual view that’s being displayed. RecyclerView will use itemView when binding an item to display on the screen, when drawing decorations around the view like a border and for accessibility. RecyclerView does not care what kind of view is stored in itemView. You can put anything here like the TextView or even a constraint layout. We can also see some useful methods like getAdapterPosition - that the RecyclerView can use to figure out the position in the list that was bound to a particular ViewHolder. Another important one is getLayoutPosition - which RecyclerView can use to know in what position the ViewHolder was displayed. 

Your ViewHolder can tell RecyclerView what its ID is. and ID is just a unique identifier like night ID on our sleep night. When we override getItemId, RecyclerView can use this ID when performing animations.  

The most important part is to note that a ViewHolder is a key part of how RecyclerView actually draws, animates, and scrolls the list. If it just used regular views, it wouldn’t be able to easily keep track of where on the screen they were displayed. And it would not be able to animate changes as well. 

——
### Implementing a ViewHolder
Steps:
1. The first step is to create a layout file that the ViewHolder will hold. 
2. Second create the ViewHolder
3. Use the view holder in the adapter.
Reference link: https://classroom.udacity.com/courses/ud9012/lessons/ee5a525f-0ba3-4d25-ba29-1fa1d6c567b8/concepts/3314a9f7-d4c2-4b27-aceb-74544b5b742a 

——
### Drawback of notifyDataSetChanged()
- Re-draws every view
- Very expensive

### RecyclerView Updates
RecyclerView has a rich API for updating a single element. You can tell RecyclerView that an item was:
- Added
- Removed
- Move 
- Update : the content has changed.

### DiffUtil
    RecyclerView has a class called DiffUtil,  which is exactly for calculating diffs of differences between two lists. 

It will take an old list and the new list, and figure out what’s different. It will find items that were added, removed or changed. Then, it will use an algorithm called the Myers diff to figure out the minimum number of changes to make from the old list to produce the new list. That means, instead of adding an item and removing it somewhere else, it will figure out what needs to be done. 

#### Benefits:
- Only redraw changed items
- Animate by default
- Efficient 

——
### Refresh Data with DiffUtil
    Reference link: https://classroom.udacity.com/courses/ud9012/lessons/ee5a525f-0ba3-4d25-ba29-1fa1d6c567b8/concepts/9a6c1600-1f64-4acb-811a-b5d89649d4a1

### Add DataBinding to the Adapter
Using data binding to set the views. For that we need to create Binding Adapter.
Reference link: https://classroom.udacity.com/courses/ud9012/lessons/ee5a525f-0ba3-4d25-ba29-1fa1d6c567b8/concepts/f13214fb-2d67-4155-adee-ea7b36458c36

Note: to add binding to xml find we can click on the layout and option+enter to get the intention menu and select binding option which will add the boiler code for it.

——

### Using GridLayout
https://classroom.udacity.com/courses/ud9012/lessons/ee5a525f-0ba3-4d25-ba29-1fa1d6c567b8/concepts/2b5d97fd-f93a-4003-84a0-8f0054c2574e

———
### Implement a click listener 
I used setOnClickListener instead of databinding look at SleepNightAdapter.kt file for it. 

https://classroom.udacity.com/courses/ud9012/lessons/ee5a525f-0ba3-4d25-ba29-1fa1d6c567b8/concepts/a24d8fdb-64ab-419c-bb29-c1225e104fa9

——

### Navigate on click
I called database get method in SleepDetailViewModel to get the sleepNight since I was getting errors when trying to use databinding.
https://classroom.udacity.com/courses/ud9012/lessons/ee5a525f-0ba3-4d25-ba29-1fa1d6c567b8/concepts/3f63055c-ca61-4d3a-a706-cf429e454d05 

——
### Adding Headers to the RecyclerView
https://classroom.udacity.com/courses/ud9012/lessons/ee5a525f-0ba3-4d25-ba29-1fa1d6c567b8/concepts/f6f93918-d9a6-4dc7-a44e-d10532325516

I did not add header to the recyclerview. Please look at this video if you want to learn how to add header to the recycler view. 

— 

 


