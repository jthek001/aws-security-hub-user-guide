# Configuring an EventBridge rule for automatically sent findings<a name="securityhub-cwe-all-findings"></a>

You can create a rule in EventBridge that defines an action to take when a **Security Hub Findings \- Imported** event is received\. **Security Hub Findings \- Imported** events are triggered by updates from both [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html) and [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html)\.

The instructions provided here are for the EventBridge console\. When you use the console, EventBridge automatically creates the required resource\-based policy that enables EventBridge to write to CloudWatch Logs\.

You can also use the [https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutRule.html](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutRule.html) API operation of the EventBridge API\. However, if you use the EventBridge API, then you must create the resource\-based policy\. For details on the required policy, see [CloudWatch Logs permissions](https://docs.aws.amazon.com/eventbridge/latest/userguide/resource-based-policies-eventbridge.html#cloudwatchlogs-permissions) in the *Amazon EventBridge User Guide*\.

**To create an EventBridge rule for a Security Hub finding**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**\.

1. Choose **Create rule**\.

1. Enter a name and description for the rule\.

1. For **Event source**, choose **Event Pattern**\.

1. Choose **Custom pattern**\.

1. Copy the following example pattern and paste it into the **Event pattern** text area\. Be sure to replace the existing brackets\.

   ```
   {
     "source": [
       "aws.securityhub"
     ],
     "detail-type": [
       "Security Hub Findings - Imported"
     ]
   }
   ```

   This is the minimum required information for the event pattern\. It identifies the source and event type\. With this pattern, all findings match the rule\.

   If you want to filter findings that trigger this rule, then you also add a `detail` entry\. The `detail` entry contains the attribute values to use to filter the findings\.

   ```
   {
     "source": [
       "aws.securityhub"
     ],
     "detail-type": [
       "Security Hub Findings - Imported"
     ],
     "detail": {
       "findings": {
         <attribute filter values>
       }
     }
   }
   ```

   For each attribute that you provide, the format is as follows\.

   ```
   "<attribute name>": [ "<value>" ]
   ```

   To match multiple values, provide a comma\-separated list\.

   ```
   "<attribute name>": [ "<value1>", "<value2>
   ```

   For example, to only apply the rule to findings that come from Amazon Inspector, add the following filter\.

   ```
   "detail": {
       "findings": {
           "ProductArn": [
               "arn:aws:securityhub:us-east-1::product/aws/inspector"
           ]
       }
   }
   ```

1. Choose **Save** to save the pattern\.

1. Under **Select targets**, select and configure the target to invoke when this rule is matched\.

1. Choose **Create**\.