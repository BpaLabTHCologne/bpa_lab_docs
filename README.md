# bpa_lab_docs 
## About the BPA lab
The Business Process Automation Lab (BPA Lab) at the TH Cologne is a small-sized and modular model factory focused on business process automation and analytics. 

The BPA Lab has two main goals: 

1) Demonstration: Demonstrate concepts and technologies for business process automation and analytics. To this end, various software components such as Business Process Management Systems (BPMS) and Robotic Process Automation (RPA) are combined with hardware components like a Fischertechnik I4.0 factory, a Fischertechnik warehouse robot and IoT devices to simulate the production and logistics processes of a bicycle manufacturing company. In addition, technologies such as process mining will be used to demonstrate how business processes can be analysed and improved through a data-driven approach. 

2) Teaching: Individual building blocks of the BPA Lab (like the Fischertechnik warehouse robot) will be used in student projects (Bachelor) as building blocks for self-defined process implementations to teach the aspect of process-oriented application integration in a tangible way. 

![BPALab_concept](Updated_graphics/Conceptional_Architecture.png "BPA Lab concept")

## About this repository 
This repository contains documentation about the implementation of the BPA Lab as a Demonstration Factory. The demonstration scenario is an end-to-end bicycle ordering, manufacturing and shipment process.
This process has been implemented with Camunda 8 (BPMS) as the central BPMS and multiple other software components.

Remark: Please refer to the bpa_lab_student_docs repository (wiki) for documentation on how to use individual  modules of the BPA Lab (e.g. warehouse robot) inside other process implementation projects ([bpa_lab_student_docs](https://github.com/BpaLabTHCologne/bpa_lab_student_docs/wiki)).

The following sections provide:
- a high level overview of the solution
- a description of the business scenario implemented in the BPA Lab
- a description of the software architecture

## About the solution 
The architecture and implementation are still under development. The architecture covers the following three levels (see simplified overview):
- Controller: control of hardware components (which are: FT I4.0 factory, FT robots, IOT devices)
- Process applications (job workers) communicating via gRPC to Camunda 8 and via MQTT to controllers
- (BPMS) Workflow engine: Camunda 8 (self-managed) platform executing various processes and decision models of the process applications

A modular design following a Domain Driven Design approach is envisaged to allow independent and parallel work on student projects in the factory. It should also facilitate the use of building blocks in student projects. 
Therefore, one of the criteria for splitting job workers and process models is the business domain (order management, manufacturing, shipping, purchasing, logistics...). 

### Simplified overview 

![simplified overview](BPALab_Simplified_Arch_240924.png "Simplified overview")

### Implementation of the demonstration factory

The latest version of the BPALab as a Demonstration Factory implementation is available in the following repository: 
([bpa_lab_demonstration_factory](https://github.com/BpaLabTHCologne/bpa_lab_demonstration_factory))

This implementation uses docker.  

## Business scenario of demonstration factory (strategic description)

The following description depicts the business process in the BPA Lab for Demonstration on a strategic (non-technical) level. It is - work in progress - and requires improvements (harmonization of terms, more details, ...)

The scenario: A bicycle manufacturer offers highly customizable bicycles to consumers 

![process landscape](Updated_graphics/process_landscape.png "Process landscape")

The entire end-to-end process is managed by the order management process, which initiates purchasing, manufacturing, and shipping processes as needed. 
Purchasing and manufacturing use the Fischertechnik Industrie 4.0 Factory to post goods receipt of components and for the actual manufacturing steps. In addition, order management and shipping use the distribution warehouse (single Fischertechnik robot) to post goods receipts and goods issues for finished goods.

The following diagrams provide strategic process models for each process. The process models do not include any technical aspects and only illustrate the happy path.

### Order management and production control

![order_management](Updated_graphics/order-management-process.png "Order management")

This process manages the fulfillment of customer orders.

The process starts when a customer order is entered into a web portal (external web application), which triggers the end-to-end order management process. During the process, the availability of the ordered bike is checked. If a bike with the exact specifications is in stock, the shipping process is initiated. After shipping, an invoice is sent to the customer and the process is complete.

In the standard scenario, the ordered bicycle is not in stock. A production order is created in the production control process. The production order specifies the required components based on a bill of materials. If any component is missing, the purchasing process is initiated. Otherwise, the manufacturing process is initiated immediately. Once the product is manufactured, shipping is initiated.

### Purchasing

![purchasing](Updated_graphics/purchasing_process.png "Purchasing")

This process manages the purchase of required components. The process starts when a component is needed. 

First, a purchase order is created that specifies parameters such as vendor, material, and quantity. The system then sends the purchase order to the vendor and waits for confirmation from the vendor. The materials are then stored and the stocks are updated.  

The process then marks the material requirement as fulfilled and the purchasing order as completed.

### Manufacturing

![manufacturing process](Updated_graphics/ManufacturingProcess.png "Manufacturing process")

The process manages the physical production simulation of the FT factory. The process is started after a production order has been created in the production control process and sufficient materials are available to manufacture the product.

An order must be placed that leads to the production of a new bicycle using the specified material. Before this, however, a check is made to see whether a physical workpiece is already available in the Ft factory's warehouse. If not, a physical workpiece must first be transferred to the FT factory so that the control program can store and recognize it. Once the bicycle has been produced from the material (workpiece), the process is complete.

### Shipment

![shipment process](Updated_graphics/shipment-process.png "Shipment process")

This process manages the shipment of finished goods to the customer. The process begins when a product is in stock and ready to be shipped to the customer.

First, the shipping data is checked. The distance and duration of the shipment from the warehouse to the delivery address is then measured. The Openrouteservice API is used for this. The logistics process is then started to receive the customer order to be shipped from the warehouse. A message is then sent to the customer that the order is now being dispatched. The process, and therefore also an end-to-end process instance, is completed when the order is handed over to the customer.

### Logistics

![Logistics process](Updated_graphics/warehouse-operations-process.png "Logistics process")

This process manages the flow of finished goods in a distribution warehouse. This is represented by the Fischertechnik warehouse robot.
The process begins when a transfer order for the warehouse is received by another business. This could be an instruction to move products in or out of the warehouse.

There are two scenarios:
"Put-away (store)": This means a product needs to be stored in a specific storage location.
"Picking (exstore)": This indicates that a product need to be retrieved from a specific storage location.

If the transfer order is for "put-away (store)," the system checks if the specified storage location is actually available in the warehouse to store the products. This check is done by the warehouse staff. If the storage location is available, the process stores the product in the warehouse by calling the warehouse robot control module. If the storage location is not available, the warehouse staff needs to specify an alternative storage location. After storing the product, the system updates the information about the warehouse's occupancy status in the inventory database.

If the transfer order is for "picking (exstore)," the process involves checking the actual availability by the warehouse staff. If product is available the requested product are picked from the warehouse by calling the warehouse robot control module. After retrieving the product, the system updates the occupancy status of the storage location in the inventory database. 

The process then marks the transfer order as completed, indicating that the requested action (storage or retrieval) has been successfully executed.

## Overview of data and sources

The following graphic shows the data sources of the BPA Lab system and their data.

![data_overview](Updated_graphics/data_overview.png "Data overview")

The next diagram shows the concrete data model of MySQL with all entities and attributes of the current implementation.

![mysql_data_model](Updated_graphics/mysql_data_model.png "MySQL data model")

## Requirements and software architecture (work in progress)

### Requirements
The following set of requirements has been gathered at the beginning of the project. It serves as the basis for the definition of a software architecture of the BPA Lab (work in progress):

R1: Independent operation of the components
The individual components of the system should be able to run independently of each other.

R2: Integration of external IT systems
The system should allow seamless integration with existing IT systems, e.g. integration with the web application for order management or and Process Mining solution. 

R3: Clear and distinct responsibilities
The responsibilities of the individual components should be clearly defined and distinguished.

R4: Easy expandability
The system should be easily expandable with further self-developed components.

R5: Interchangeability and further development of the components
The components of the system should be independently further developable and interchangeable.

R6: Comprehensive and flexible process data acquisition and provision
Process data should be able to be comprehensively captured. Data should be stored and flexibly made available for various analyses.

R7: User friendliness (operator perspective)
The user interface of the system should be user-friendly to ensure a positive user experience.
The user interface should be intuitive and easy to understand.

R8: Easy installation and low infrastructure requirements (developer perspective)
The system should be easy to install to allow students to host instances of the system.

R9: Robustness
The system architecture should be robust to minimize failures and ensure reliable functionality.
The components should run stably under various operating conditions and avoid unexpected failures.

R10: Maintainability and operation
The system should be easy to maintain.

### Software architecture (work in progress)

The following C4 diagrams (https://c4model.com/) show the context and containers (not equal to the docker container) of the BPALab software architecture. Aspects are still under discussion and may be changed.

![C4 context diagram](C4architecture/c4-contextDiagram.drawio.png "C4 context diagram")

![C4 container diagram](Updated_graphics/C4_System_Architecture.png "C4 container diagram")

### Architecture questions and decisions (work in progress)

In this section few questions on architecture raised during a later stage of the BPALab project and the respective decisions have been recorded (not complete)

Decision 1: MQTT broker for communication between process applications and controller (decision done)
Justification: decoupling, protocol common in IOT field

Decision 2: Model inter-process communication in BPMN by Message Send Tasks and Message Receive Task (decision done)
Justification: Tasks offer use of boundary events e.g. to handle errors triggered by job workers

Decision 3: Messages between processes and process application - see manufacturing prototype (decision done)
Justification: to have simple solution and be aligned with process application - process control communication

Decision 4: Using the self-managed version of Camunda 8 and Docker compose Camunda 8 core  (decision done)
Justification: avoid interferences between members during development phase; development environment identical to production environment; note: performance with docker and number of containers to be verified

Decision 5: Use JobWorker to send emails (decision done)
Justifications: issues with SendGrid Connector; existing solution in project

Decision 6: Use JobWorker for database transaction and mysql database in docker container  (decision done)
Justifications: existing solution in project

