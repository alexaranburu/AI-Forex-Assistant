# AI Forex Assistant
This repository contains a step-by-step guide of how to create an AI Forex Assistant using Microsoft Power Platform tools like Power Apps and Power Automate, making use of latest Generative AI technology such as Copilot. The goal is to create a user dashboard where a foreign exchange pair evolution is displayed, together with supporting data.

In this specific case and for illustration purposes, the selected foreign exchange pair has been the CHF/EUR pair. In addition, historical Gross Domestic Product (GDP) growth rates and interest rates of Switzerland and the European Union are used as supporting data. Not only that, but the user can also ask the AI Forex Assistant to predict next month's exchange rate. Thanks to a trained Machine Learning model on historical data, the AI Forex Assistant is then able to estimate the future value considering market trends.

The user interface and dashboard of the application can be visualized in the following image:<br><br>
![Image](/images/dashboard_final.PNG)

## Index
This guide follows a series of steps to complete the development of the AI Forex Assistant application:

**1.- Create and populate Data Tables using Copilot in Power Apps.**

**2.- Design dashboard user interface with the help of Copilot in Power Apps.**

**3.- Generate and train a Custom Prediction Model in AI Builder.**

**4.- Create a flow in Power Automate with Copilot to predict future values.**

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

1- "Please add 46 more rows."

2- "The month column should go from 01/08/2019 to 01/10/2023."

3- "Convert the Month column into text. The values should display the month and the year only."

After this conversation, the table should look like this:

![Image](/images/copilot_table_2.PNG)

At this point, once we populate the table with the correct historical data, we can then create the app and continue with the user interface design. Note that the historical data has been collected from trusted sources such as the European Central Bank (ECB) and the Swiss National Bank (SNB). The table is stored as a Dataverse table in Power Apps.

<br><br>
## 2.- Design dashboard user interface with the help of Copilot in Power Apps
Now that the app has been created in Power Apps, we need to work on the user interface of the main screen. By default, the app will have a data table screen at first:

![Image](/images/dashboard_1.PNG)
<br><br>
So the first thing we need to do is to remove that data table screen and add the line charts of CHF to EUR, GDP Growth and Interest Rate. For that, we are going to delete the ScreenContainer1 element on the left pane and then ask Copilot how we can add our three line charts of interest, with the prompt "Suggest me how to add a line chart". Note that once we follow Copilot's instructions, we have to customize the data, title, legends and color of each chart:

![Image](/images/dashboard_2.PNG)
<br><br>
Next, we are going to ask Copilot to Add a button with a text that says "Predict Future Exchange Rate". Likewise, we are going to conveniently let Copilot add three text labels. After placing the new elements in their respective positions and customizing their text format, we are going to create a global variable called varFutureExchangeRate, which will be linked to our predicted future exchange rate value. The final dashboard user interface should look like this:

![Image](/images/dashboard_3.PNG)
<br><br>
<br><br>
## 3.- Generate and train a Custom Prediction Model in AI Builder
We are now finished with the design of the dashboard user interface and we can move on to the next step, which involves generating and training a Custom Prediction Model in AI Builder inside Power Apps. If we go to the AI Hub section from the Power Apps homepage and select the AI models, we are presented with a list of prebuilt and custom models that can be used within Power Apps and Power Automate:

![Image](/images/model_1.PNG)


For this use case, we are going to select and create a "Predict future outcomes from historical data" custom model. The difference between prebuilt models and custom models is that custom models need to be trained with your own dataset. For the purpose of creating this guide, we are going to use our very simple dataset, consisting of 51 rows of historical data (the minimum number of samples for any custom models is 50). Please refer to the disclaimer to read more about the model employed for this guide.

As a first step, we must select the CHF to EUR column in our Currency Data table as the historical outcomes the model should study to predict future values. Next, we are going to select which are the feature inputs of the model that will influence the prediction. In this case, we are going to select the supporting historical data that is directly correlated to future exchange rates, that is, the CH and EU GDP Growth, together with the CHF and EUR Interest Rates. Do not forget to include the time context of our data by including the Month column as well:

![Image](/images/model_2.PNG)

![Image](/images/model_3.PNG)
<br><br>
After that, we can skip the filtering step and continue with the model training. With such a small dataset, the training should not take more than 5 minutes. Finally, when the training ends, we need to publish the model if we want to access it in Power Automate. Note that in case we want to change the settings of the published model anytime, we can always do it and retrain the model to adapt to the new configuration.

![Image](/images/model_4.PNG)
<br><br>
<br><br>
## 4.- Create a flow in Power Automate with Copilot to predict future values


<br><br>
## Disclaimer
The purpose of this guide and applicaton is not to predict future foreign exchange values as accurately as possible, but to focus on the steps followed and functionalities available in Power Apps and Power Automate with the assistance of Generative AI, via Copilot. The author is aware that the predictor model is likely to underfit due to the size of the training dataset. Accuracy and reliability of the prediction could be improved in several ways (using a larger training dataset to avoid underfitting, increasing decimal places on data, adding extra supporting data such as Income Growth Rate, connecting the data tables to an API that provides real-time financial information, etc.), but that is out of the scope of this guide.
