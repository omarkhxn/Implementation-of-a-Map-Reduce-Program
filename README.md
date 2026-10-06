# Top 3 Cities by Average Credit Card Spend (Hadoop MapReduce)

A two-stage Hadoop MapReduce program that analyses credit card transactions from India. It calculates the average amount spent in each city and finds the top 3 cities with the highest average spend.

Built for a university module on High Performance Computational Infrastructure.

## Dataset
- Source: [Analyzing Credit Card Spending Habits in India (Kaggle)](https://www.kaggle.com/datasets/thedevastator/analyzing-credit-card-spending-habits-in-india)
- Fields: index, city, date, card type, expense type, gender, amount
- Preprocessing: the `, India` suffix was removed from the City column. The cleaned file is used as `CreditCard2.txt`.

## How it works

**Job 1: City-wise average**
- `CreditMapper` reads each CSV line and emits `(city, amount)`.
- `CreditReducer` aggregates the amounts for each city (sum and count) to compute the average.

**Job 2: Top 3 cities**
- `CreditMapper2` passes Job 1's output through unchanged.
- `CreditReducer2` keeps the 3 highest averages in a `TreeMap` and writes the final result in `cleanup()`.

`CreditDriver` chains the two jobs together.

## Output
- Job 1: `City_name : Average_amount_spent` for every city
- Job 2: `The Top 3 cities with highest average spent is: ...`

Result: Thodupuzha (~296,684), Nahan (~264,597) and Alwar (~263,489).

## Run
```bash
hadoop jar <your-jar-name>.jar CreditDriver <input-path> <job1-output> <job2-output>
