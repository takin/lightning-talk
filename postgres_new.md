# AI Tools For Rapid Data Modeling and Analytics
This talk will present how we can leverage [postgres.new](https://postgres.new) to help us speedup the data modeling process just by using the Natural Language Prompts.

It also can be use to do data analytics just by dumping the CSV file into it. Then we can prompt it to do analytics and what is the most intersting things is that it can display the result in charts :-)


## Prompts Example

### [Part 1] Modeling The Data
I want to create a booking apps where the user can book sport venues like badminton, tennis, gym etc. also the user can do the booking payment in the app

translate this SQL DML statement into golang struct. Remember, do not create the main function. here is the statement

### [Part 2] Analyzing the Data
List top 5 customers with the most transactions amount for this year. Note that the valid transactions is only the one with paid, done or reconciled status only. Display it in bar chart

Now i want the transaction count of that customers. Also display it in bar chart

Next, I want to know total success transaction amount group it by the payment channel name. Success transactions means the transactions with paid, done or reconciled status only. Also, Don't forget to show it in bar chart

Please merge "Bank BCA" with "BCA" as it is considered as the same channel

Okay, now i wanted to know the distribution of the transactions based on the company product code. I want you to show it in pie chart

Okay, it is interesting to know EMBRL further, now please show me the transaction disribution of the company code EMBRL based on the payment channel name. Don't forget that "Bank BCA" and "BCA" considered as the same payment channel, also display the result in pie chart

Okay, now I wanted to know the total transaction amount trends for 2024 group it by the month. Remember that only include the paid, done and reconciled transactions only. Display it in line chart

Ok now, i want to see the transactions count trends for 2024 month by month. Also display it in line chart

Nice!. Based on the last result, can you give me how much the tranction growth compare to previous month in percent