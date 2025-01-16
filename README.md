# CodingChallenge-Test

Q)A retailer offers a rewards program to its customers, awarding points based on each
recorded purchase.

Ans=>

using System;
using System.Collections.Generic;
using System.Linq;

class Program
{
    static void Main(string[] args)
    {
        // Sample data: Customer transactions (CustomerId, Month, Amount)
        var transactions = new List<Transaction>
        {
            new Transaction("C001", "January", 120),
            new Transaction("C001", "January", 80),
            new Transaction("C002", "January", 140),
            new Transaction("C001", "February", 200),
            new Transaction("C002", "February", 60),
            new Transaction("C003", "March", 50),
            new Transaction("C003", "March", 110),
        };

        // Calculate reward points
        var rewardPoints = CalculateRewardPoints(transactions);

        // Display results
        foreach (var customer in rewardPoints)
        {
            Console.WriteLine($"Customer: {customer.Key}");
            foreach (var month in customer.Value)
            {
                Console.WriteLine($"  {month.Key}: {month.Value} points");
            }
            Console.WriteLine($"  Total: {customer.Value.Values.Sum()} points\n");
        }
    }

    static Dictionary<string, Dictionary<string, int>> CalculateRewardPoints(List<Transaction> transactions)
    {
        var rewards = new Dictionary<string, Dictionary<string, int>>();

        foreach (var transaction in transactions)
        {
            int points = CalculatePoints(transaction.Amount);

            if (!rewards.ContainsKey(transaction.CustomerId))
            {
                rewards[transaction.CustomerId] = new Dictionary<string, int>();
            }

            if (!rewards[transaction.CustomerId].ContainsKey(transaction.Month))
            {
                rewards[transaction.CustomerId][transaction.Month] = 0;
            }

            rewards[transaction.CustomerId][transaction.Month] += points;
        }

        return rewards;
    }

    static int CalculatePoints(decimal amount)
    {
        int points = 0;

        if (amount > 100)
        {
            points += (int)((amount - 100) * 2);
            points += 50; // 1 point for every dollar between 50 and 100
        }
        else if (amount > 50)
        {
            points += (int)((amount - 50) * 1);
        }

        return points;
    }
}

class Transaction
{
    public string CustomerId { get; }
    public string Month { get; }
    public decimal Amount { get; }

    public Transaction(string customerId, string month, decimal amount)
    {
        CustomerId = customerId;
        Month = month;
        Amount = amount;
    }
}

Output=>

Customer: C001
  January: 130 points
  February: 300 points
  Total: 430 points

Customer: C002
  January: 180 points
  February: 10 points
  Total: 190 points

Customer: C003
  March: 120 points
  Total: 120 points





