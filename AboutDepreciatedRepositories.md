# bpa_lab_docs 

The BPA Lab is developed in an iterative approach and combines the result of multiple (student) projects. As a consequence, the GiHub Organization contains a number of depreciated repositories.  

## About depreciated repositories in BpaLabTHCologne organization for building blocks

The multiple building blocks of the BPA Lab were developed in multiple repositories. Besides the repositories for documentation (this one, [bpa_lab_student_docs](https://github.com/BpaLabTHCologne/bpa_lab_student_docs/wiki) and [bpa_lab_papyrus_uml](https://github.com/BpaLabTHCologne/bpa_lab_papyrus_uml)) there are multiple other repositories for components.

### Controller level: 

bpa_lab_txt_warehouse_control: Control component for the Fischertechnik warehouse robot

bpa_lab_ft_factory_control: Control component for the Fischertechnik Learning Factory 

### Communication between Controler and Process Applications level:

bpa_lab_broker: Info on MQQT setup; required for communication of controller and job worker 

### Process application level:

The process applications do include smaller process models for testing. These are not yet integrated with each other to implement the envisaged end-to-end business processe of a bicycle manufacturing company.

[bpa_lab_warehouse_operations](https://github.com/BpaLabTHCologne/bpa_lab_warehouse_operations): process application (Zeebe/Camunda 8 job worker) communicated via MQQT and via gRPC with Camunda 8 (Cloud). Implementation includes simplified business process, databases etc.

[bpa_lab_ft_factory_proto_processapplication](https://github.com/BpaLabTHCologne/bpa_lab_ft_factory_proto_processapplication/tree/main/src): process application (Zeebe/Camunda 8 job worker) communicated via MQQT with [bpa_lab_ft_factory_control](https://github.com/BpaLabTHCologne/bpa_lab_ft_factory_control)

## About depreciated repositories in BpaLabTHCologne organization for student projects

In addition, this GitHub organization contains a number of depreciated repositories containing the results of student projects between 2021 and 2023. Some of these do not follow the new (2023) architecture of the BPA Lab solution. However, all these projects contain valuable parts that are (partly) not integrated into the new solution

[BPALab_22_Prototype](https://github.com/BpaLabTHCologne/BPALab_22_Prototype): Very initial prototype for integrating a Camunda with an IOT device and a Fischertechnik warehouse robot. It is based on Camunda 7 and no containers (docker) are used. All ideas/aspects are re-used/re-worked in other repositories.

[BPALab_GP22_23](https://github.com/BpaLabTHCologne/BPALab_GP22_23): Result of the Camunda (and RPA) part of the Guided Project WS 22/23. Contains valuable parts (overall process design for bicycle manufacturing, single page application for order entry connected to Camunda, RPA demonstrator for shipping). It introduces a docker setup. Solution based on Camunda 7 and without MQQT. Results can be considered as a design document or in some cases as building blocks for the new solution.

[BPALab_22_Prototype_IOT](https://github.com/BpaLabTHCologne/BPALab_22_Prototype_IOT): Results of Guided Project WS 22/23 for IOT solution with integration to Camunda. It is not integrated with Camunda / RPA deliverable and was (partly) used in the master thesis IoT-aware-BP. It contains the most sophisticated IOT device logic.

[BPALab_master_thesis_IoT-aware-BP](https://github.com/BpaLabTHCologne/BPALab_master_thesis_IoT-aware-BP): Master thesis at BPA Lab, focusing on how to integrate process and IoT data in terms of architecture, process modeling, etc. Implementation based on Camunda 8 concepts will be used for future design. Integration of warehouse robots in business process (manufacturing and shipping) is considered. IOT device setup must be combined with results from BPALab_22_Prototype_IOT (which uses more sophisticated setup).  
