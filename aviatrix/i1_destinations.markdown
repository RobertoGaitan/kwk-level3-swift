# Aviatrix I1: Destinations

As you can see in the `main.swift`, our plane should be able to fly places. But right now, all it can do is stay on the runway in St. Louis. Let's build up the functionality to move between cities.

## What We're Going For

When this iteration is done, we should be able to fly between known cities. When we run the program and type in `b` to fly, we will see an output something like this:

```
Where would you like to fly to?

0) St. Louis (0 miles)
1) Phoenix (1260 miles)
2) Denver (768 miles)
3) Salt Lake City (1150 miles)

Destination Number:
```

## The Data

Open the `AviatrixData` file and check out the data that's in there. You should find:

* `fuelPrices` - in a later iteration we'll work on purchasing fuel. At that time we'll need this collection of fuel prices.
* `knownDistances` - this is a huge dictionary that defines the distances between the supported cities where each city marker points to another dictionary. The second-level dictionary has the cities you could fly to along with the distance to get there.

## Building the Menu

Right now when we type in `b` for the fly menu we just see:

```
Action: b

Where would you like to fly to?

0: St. Louis () miles
```

Let's work on displaying more destinations.

### Looking at `main.swift`

The `fly` method in `main.swift` is what's outputting this menu. It's some of the more complicated code in the `main.swift` file, but we can pick it apart piece by piece.

On the third line of the `fly` function we can see it's calling `myPlane.knownDestinations` and below, iterating over that array. We need to modify the return value of `knownDestinations` function (which lives in `Aviatrix`).

_Let's take a minute to talk as a group about how we are calling this function that belongs to the `Aviatrix Class`._

### `knownDestinations`

Remember that all the data about destinations is in `AviatrixData`. We _want_ to have the `knownDestinations` function return an array like this:

```
["St. Louis", "Phoenix", "Denver", "SLC"]
```

*Can you write the code so that the `knownDestinations` function returns just the keys from that dictionary?*

Stuck? Remember, to access the data from `AviatrixData`, we have to have an instance of it. Also, look back at the Dictionary slides if you need to refresh on how to access keys.

#### Results

If you get that function right and run the program, you'll get output like this:

```
Action: b

Where would you like to fly to?

0) St. Louis (0 miles)
1) Phoenix (0 miles)
2) Denver (0 miles)
3) Salt Lake City (0 miles)
```

We've got the city names and next want to add in the distances.


### Distance to Destination

In `main.swift` we can find the line that outputs that distance. It relies on the variable `distance` that was created here:

```
distance = myPlane.distanceTo(target : String)
```

So to get the distance to display correctly we've got to look at the `distanceTo` function in our code (in `Aviatrix`).

#### `distanceTo`

If you look in `AviatrixData` you'll see that the distances are in there, but how do we get to them?

First, we need to know where we are. By default, our plane stats in St. Louis. So for a first round, let's assume that any distance being calculated is *from* St. Louis.

The distance from St. Louis to Phoenix is 1260 miles. You can see that number inside the `knownDistances` function of `AviatrixData`. To access this data, we could use the following code:

```
knownDistances["St. Louis"]!["Phoenix"]!
```

The `knownDistances` variable is a dictionary. `St. Louis` is a key in that dictionary. It points to a value which is *another* dictionary.

`"Phoenix` is a key in that second dictionary. It points to a value 1260, which is what we're looking for.

So the code `knownDistances["St. Louis"]!["Phoenix"]!` gets us the distance between St. Louis and Phoenix. When we look at our `distanceTo` function we have an argument named `target` that is the city we're heading to.

*Write code in your `distanceTo` function that returns the distance between St. Louis and your `target`.* Re-run the program and check the output you see in `main.swift` against the numbers in `AviatrixData`.

#### Planning for the Future

You probably ended up with a line of code that looked like this:

```
return data.knownDistances["St. Louis"]![target]!
```

That'll work great as long as our current location is St. Louis. But we're about to start moving around. How can you modify this line so that it works based on our _current_ location?


Stuck? 
- You will need an instance variable in `Aviatrix` that holds a string with the current location. Let's call this variable `location`.
- The `distanceTo` function needs to take another argument (the instance variable that holds the current location) so we don't have to hard-code a certain city in.

Once this is done, let's update the message in `fly` in `main.swift` that says:

```
print("🛬 You've arrived in ________________!")
```

... so that it actually prints out the plane's current location.

#### Results

When you've got it right and run your program, your fly menu will look like this:

```
Action: b

Where would you like to fly to?

0) St. Louis (0 miles)
1) Phoenix (1260 miles)
2) Denver (768 miles)
3) Salt Lake City (1150 miles)
```

Try flying to Denver -- you'll get this message: "🛬 You've arrived in St. Louis!" Hmm... I thought we just flew to Denver? Looks like we have some more work to do.

### Flying

We're displaying the menu of destinations. We're allowing the user to pick a destination. But our plane isn't moving.

#### Understanding `main.swift`

Back in `main.swift` look at the `fly` function. Inside the 'if' statement, you'll find some lines that looks like this:

```
desiredLocation = myPlane.knownDestinations()[response!]
...

myPlane.flyTo(destination: desiredLocation)
```

Let's break that down:

* The `response` here is the menu option number that the user typed in, like `2` for Denver
* `knownDestinations` is an array, the return value from the function `knownDestinations` that we wrote in `Aviatrix`
* `knownDestinations()[response!]` when the `menu option number` is `2` is like `knownDestinations[2]` which picks out the marker `"Denver"`
* The `desiredLocation` variable will store a string, of the city that was selected by the user
* So when our instruction runs it's really like `myPlane.flyTo("Denver")`

We'll need to work on that `flyTo` function.

#### `flyTo`

In Aviatrix, find the `flyTo` function. It looks like this:

```
func flyTo(destination : String) {

}
```

The `destination` that comes in is the city like `"Denver"`.
*Can you write one line here that stores that city into the plane's `location` instance variable?*

Test it by running `main.swift` - hopefully you are in the correct city🤷‍♀️.


#### Location Update in Gauges

In `main.swift`, there is a `gauges` function that is just printing out some information when the user types in `a`. Uncomment the line that says:

```
print("| Location:  | \(myPlane.location)")
```

Now, run your program and type in `a`.

NOTE: You will need to continue doing this as you create instance variables, and those instructions won't be written into the lab🤓 You got this.

## What We've Got

### After this work, when you run the program:

* The `gauges` info that prints out is still mostly blank, but our `Location` is accurate
* Flying to a different city works and we can move from city to city
* Refueling doesn't do anything yet

### Commit Your Changes

In Xcode, select `Source Control`, `Commit`, type in a message describing your changes (probably "Complete iteration 1"), then click `Commit Files`.

Make sure your code works like that before you move on to [I2: Distances](./i2_distance_and_fuel.markdown).

### EXTENSION: Dream Destinations

Can you add a destination to the application? You'll need to carefully change `AviatrixData`:

* Make up a gas price and add it to the `fuelPrices` function
* The hard part is `knownDistances`. Your best bet is to carefully copy and paste one of the existing dictionaries. You'll need to add your destination to each of the existing lists. Take extra care to add commas where needed!

If you'd like to use realistic distances, you can [use this online air mileage calculator](http://www.webflyer.com/travel/mileage_calculator/).

If you get one new destination to work correctly, feel free to add a couple more!

### Extra Challenge

Was flying around too easy for you? How about a tricky challenge?

It would be nice if, when the program listed out the possible destinations, they were ordered by distance from the current city.

When we're in St. Louis we would currently see:

```
Where would you like to fly to?

0) St. Louis (0 miles)
1) Phoenix (1260 miles)
2) Denver (768 miles)
3) Salt Lake City (1150 miles)
```

But if this ordering were in place we'd see:

```
Where would you like to fly to?

0) St. Louis (0 miles)
1) Denver (768 miles)
2) Salt Lake City (1150 miles)
3) Phoenix (1260 miles)
```

You'll have to change code in `main.swift` to make this happen. Can you figure it out?
