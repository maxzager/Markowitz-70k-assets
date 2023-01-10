
# Markowitz simulation with 70.000 funds
## About this project

I did this project as a challenge for my Master's degree.

The objective was to find the efficient frontier and the maximum sharpe portfolio using a simulation with a dataset of **70 thousand investment funds.**

## Dataset
The dataset was provided by the professor for the challenge and it's composed by 70.000 assets (funds) and one year of daily data.

## Data challenges

### - The assets do not have the same trading days, how do I homogenize them?

I created a pivot table with the funds as columns, by their isin number, the dates as index, and the nav as value.
The pivot table homogenizes the dates by filling with Nan where we have no data.

### - There are days when most of the assets are not quoted, what do I do?

I delete the rows that are weekends.

Only 431 funds (out of 68342) report "nav" on the weekend, which added 98 rows with Nan to the rest of the dataframe, so I decide to remove these so as not to have almost 30% "made up" data with a fillna in the rest of the funds in the dataframe.

### - How many assets do I keep to work with?

I deleted the assets that have more than 20% of their data empty.

**We are left with 57683 assets** by cleaning up those with more than 20% of nan.

## Techincal challenges

### - How do I generate the random numbers and how many do I generate?

Here it's important to work with **vectorial programming**, to optimize the performance.

I define a function to generate the random numbers and the selection of random assets.

I generate a matrix of random numbers (uniform distribution), with rows equal to the number of simulations and columns equal to the number of assets to choose.

I generate a random matrix of identical dimensions of zeros or ones (uniform distribution), which I then multiply with the previous matrix, to incorporate the **possibility of assets having zero weighting**. Which is of prime importance in this problem.

I also generate a matrix of the same dimensions, with the random choices of funds.

### - How many assets do I select in each simulation to give them weight, and how do I select them?

I define a function to generate the simulations, where I pass by parameter a list of the (maximum) amount of assets that it will be able to take in each iteration. 
That is, maximum because there is also the process of assigning zeros, which lowers the amount of assets with effective weights even though they have been theoretically "selected".

In the presented code, **I used selections of assets taken by 5, 10, 15 and 20.**

The assets then are selected randomly, starting with the possibility of choosing among all the available assets and then in an iteration process will be filtered with a decay function, those that contribute to the portfolios of higher sharpe ratio (those closer to the efficient frontier). 

The idea of filtering is also to save computational power from the "learning" of the best assets, in order to simulate more mean-variance combinations only among them.



### - How many simulations am I going to perform?

Almost **4 million simulations.**

It is not trivial, because I start with 300,000 simulations, whose result I then use in the decay function to take the best assets by sharpe, reaching a total of 740,000 simulations (after 10 iterations with a function n/(i*2) ). 
Then I repeat this process 4 times (to take 5, 10, 15 and 20 assets).

After 3 million simulations, I am left with the best 20 assets (the largest of the list passed by parameter), and the function runs again the same process but starting from 100,000 simulations, thus performing 1 million more.



## Screenshot

![Frontera eficiente](https://user-images.githubusercontent.com/121939304/211667031-89fea53a-4bcd-4b52-9380-044855ec9ff5.jpg)


