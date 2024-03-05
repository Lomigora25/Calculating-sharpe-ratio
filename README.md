# Calculating the Sharpe ratio for a portfolio

import statistics

# Stock data - user input limited to 10 stocks 
stocknames = ["META", "Alphabet", "Amazon", "Apple", "Exxon Mobil", "Johnson & Johnson", "The Coca-Cola Company", "UPS", "Duke Energy Corporation", "American Tower Corporation"]

stockprices = {
    "META": [240.32, 264.72, 286.98, 318.6, 295.58, 299.89, 300.95, 326.8, 353.58, 389.73, 486.53, 484.66],
    "Alphabet": [108.22, 123.37, 120.97, 133.11, 137.35, 131.85, 125.3, 133.92, 140.93, 141.8, 140.1, 137.48],
    "Amazon": [105.45, 120.58, 130.36, 133.68, 138.01, 127.12, 133.09, 146.09, 151.94, 155.2, 173.54, 173.49],
    "Apple": [169.68, 177.25, 193.97, 196.45, 187.87, 171.21, 170.77, 189.95, 192.53, 184.40, 182.63, 181.48],
    "Exxon Mobil": [118.34, 102.18, 107.25, 107.24, 111.19, 117.58, 105.85, 102.74, 99.98, 102.81, 104.03, 104.17],
    "Johnson & Johnson": [149.0,145.1, 151.1, 155.0, 149.1, 147.8, 139.3, 138.8, 141.1, 147.0, 148.0, 148.9],
    "The Coca-Cola Company": [64.15, 59.66, 60.22, 61.93, 59.83, 55.98, 56.49, 58.44, 58.93, 59.49,60.02, 59.53],
    "UPS": [179.81, 167.0, 179.25, 187.13, 169.4, 155.87, 141.25, 151.61, 157.23, 141.9, 148.26, 148.06],
    "Duke Energy Corporation": [98.88, 89.29, 89.74, 93.62, 88.8, 88.26, 88.89, 92.28, 97.04, 95.83, 91.83, 90.86],
    "American Tower Corporation": [204.39, 184.44, 193.94, 190.31, 181.32, 164.45, 178.19, 208.78, 215.88, 195.65, 198.86, 201.76]
}

# User input
numstocks = int(input("Enter how many stocks are in your portfolio: "))

userstocks = []
stockshares = []
initialprices = []

# User stocks, shares, cost of stock 
for _ in range(numstocks):
    stock = input("Enter a stock: ")
    userstocks.append(stock)
    shares = int(input("How many shares of " + stock + " do you own?"))
    stockshares.append(shares)
    initial = float(input("How much did you buy " + stock + " for?"))
    initialprices.append(initial)

# Calculating stock returns
stockreturns = {}
for stock in userstocks:
    returns = [(stockprices[stock][i] - initialprices[userstocks.index(stock)]) / initialprices[userstocks.index(stock)] for i in range(len(stockprices[stock]))]
    stockreturns[stock] = returns

# Calculating portfolio returns
portfolioreturns = []
for i in range(len(stockprices["META"])):
    totalreturn = sum(stockreturns[stock][i] * stockshares[userstocks.index(stock)] for stock in userstocks)
    portfolioreturns.append(totalreturn)

# Calculating sharpe ratio
risk_free_rate = float(input("Enter the risk-free rate: "))
sharperatio = (statistics.mean(portfolioreturns) - risk_free_rate) / statistics.stdev(portfolioreturns)
print("You're sharpe ratio is ", sharperatio)


# Sharpe ratio explanation 
if sharperatio < 1:
    print("Having a sharpe ratio of less than 1 means that you are taking too much risk compared to the amount of return you are getting. This is because sharpe ratio shows you your return to risk so the lower it is the less return your getting for the amount of risk your taking. This means that you should consider better diverisfying your portfolio.(See our slides for a more information on how to improve your portfolio)")
elif 2 > sharperatio >= 1:
    print("Having a sharpe ratio of greater than 1 but less then 2 means that you a decent/good amount of risk compared to your returns. This is because sharpe ratio shows you your return to risk so the higher it is the more return your getting for the amount of risk your taking.This means your portfolio is pretty well diversified but it could be diversified further to further reduce the amount of risk compared to returns.(See slides for more information on how to improve your portfolio)")
elif sharperatio >= 2:
    print("Congratulations, you have a very good sharpe ratio!! (See slides for what this means and how you can maintain this)")
    
