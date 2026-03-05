The following data is sent from the process controller (Python program) of the FT factory via message broker.

1. After initialization of the Python program (one time): BPALabBroker_Starting_FTFactory_Init_Message

2. After connecting the process application to the process controller, continuous state messages are sent. BPALabBroker_After_Connection_ProcessApplication_Status_Messages

3. During the storage of a component at the Fischertechnik factory, the following messages are created: BPALabBroker_During_StoreIn_of_Components_inkl_Status_Messages

4. During the production of a product at the Fischertechnik factory (triggered by the manufacturing process application), the following messages are sent: BPALabBroker_During_StoreIn_of_Components_and_Production_inkl_Status_Messages (the file also contains the Store In period).
