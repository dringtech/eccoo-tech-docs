# care logging

The purpose of care logging is to track the care that has been given. As this represents the critical exchange of value enabled by the platform, consensus will be required between care giver and care receiver.

## Scenarios

### Agreement

In this 'sunny day' scenario, each system user will simply agree with the record defined by the appointment component.

![Care Logging - Agreement scenario](../../.gitbook/assets/care-logging-agreement.png)

1. Once the appointment time has completed, the appointment booking component sends a message which is intercepted by the care logging component.
2. The care logging component creates a pending care log entry.
3. The care log interacts with the notification component to notify the care giver and care receiver \(or whoever was at the appointment\) that they need to confirm the details of the appointment. This will initially be the fact that it took place, but may be extended to cover duration and 'satisfaction' as the platform capabilities are developed. These confirmations may occur in any order.
4. The care receiver confirm that they received the care stated in the log.
5. The care giver confirms log their agreement with the recorded details.
6. Once consensus has been logged by all participants, the care log will store the entry on the ledger, and the care logging interaction ends.

