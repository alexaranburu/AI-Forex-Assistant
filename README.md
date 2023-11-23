# AI Forex Assistant
This repository contains a step-by-step guide of how to create an AI Forex Assistant using Microsoft Power Platform tools like Power Apps and Power Automate, making use of latest Generative AI technology such as Copilot. The goal is to create a user dashboard where a foreign exchange pair evolution is displayed, together with supporting data.

In this specific case and for illustration purposes, the selected foreign exchange pair has been the CHF/EUR pair. In addition, historical Gross Domestic Product (GDP) growth rates and interest rates of Switzerland and the European Union are used as supporting data. Not only that, but the user can also ask the AI Forex Assistant to predict next month's exchange rate. Thanks to a trained Machine Learning model on historical data, the AI Forex Assistant is then able to estimate the future value considering market trends.

The user interface and dashboard of the application can be visualized in the following image:<br><br>
![Image](/images/dashboard_final.PNG)


## Index
This guide follows a series of steps to complete the development of the AI Forex Assistant application:

**1.- Create and populate Data Tables using Copilot in Power Apps.**

**2.- Design dashboard user interface with the help of Copilot in Power Apps.**

**3.- Generate and train a Prediction Model in AI Builder.**

**4.- Create a flow in Power Automate with Copilot to predict future values that can be integrated into Power Apps.**

**5.- Integrate the Power Automate flow into Power Apps and test the application.**<br><br>


## 1.- Create and populate Data Tables using Copilot in Power Apps
The first thing that we need to do is to ask Copilot what kind of App we want to build in Power Apps. In any interaction with Copilot, it is essential to make use of AI prompt engineering in order to get the best out of the trained LLM behind it. In general, there are some good practices to follow when interacting with Copilot in Power Apps and Power Automate:

* Specify both what and how. Remember to mention both what you want and how you want it.

* Support the action or instruction of your prompt with relevant content or cues that influence the action.

* Specify context. Context matters, the more you can specify like domain, topic, etc. the better.

* Provide information about the format of the output.

* Do not expect the same exact output from the same prompot always. Copilot and all LLM models are non-deterministic so the output might vary each time.

In our case, we will ask Copilot that we want to build an application to display CHF to EUR exchange rates and supporting data like CH GDP Growth, EU GDP Growth, CHF Interest Rate and EUR Interest Rate, where each value corresponds to a month:

![Image](/images/copilot_create_app.PNG)
<br><br>
As a result, Copilot generates a Data Table to meet our needs, containing random values:

![Image](/images/copilot_table_1.PNG)
<br><br>
As you can see, we get a warning message saying that "A text primary name column is required". Moreover, there are additional modifications that we want to perform on the table. One of the greatest things about Copilot is that we can have a conversation with it, iteratively interact and provide one instruction after another until we are satisfied with the result. Apart from converting the month column into the primary name column, we are also going to add more rows and modify the month value to go from 08-2019 to 10-2023. This is the sequence of the correct prompts:

1- Please add 46 more rows.

2- The month column should go from 01/08/2019 to 01/10/2023.

3- Convert the Month column into text. The values should display the month and the year only.

After this conversation, the table should look like this:

![Image](/images/copilot_table_2.PNG)

At this point, once we populate the table with the correct historical data, we can then create the app and continue with the user interface design. Note that the historical data has been collected from trusted sources such as the European Central Bank (ECB) and the Swiss National Bank (SNB).


<br><br>
## Disclaimer
The purpose of this guide and applicaton is not to predict future foreign exchange values as accurately as possible, but to focus on the steps followed and functionalities available in Power Apps and Power Automate with the assistance of Generative AI, via Copilot. The author is aware that the accuracy and reliability of the prediciton could be improved in several ways (using a larger training dataset, increasing decimal places on data, adding extra supporting data such as Income Growth Rate, connecting the data tables to an API that provides real-time financial information, etc.), but that is out of the scope of this guide.
