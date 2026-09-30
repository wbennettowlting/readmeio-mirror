---
title: Customer Onboarding Reference Data
metadata:
  robots: index
privacy:
  view: public
slug: onboarding-reference-data
---
The lists below are provided for convenience so you can quickly build dropdowns and validate input. However, because these lists change over time, we highly recommend calling the authoritative Harbor lookup endpoints in real-time to get the most accurate and up-to-date options.

### 1. Occupations — `individual.occupation`

> 📘 Authoritative API Endpoint
> `GET /api/v1/occupations`
> Submit the `value` (a slug) in address fields; Harbor maps it to the provider code at submit time.

Global list of occupations (131 options):

| `value` | label |
|---|---|
| `legislators_and_senior_officials` | Legislators and Senior Officials |
| `managing_directors_and_chief_executives` | Managing Directors and Chief Executives |
| `business_services_and_administration_managers` | Business Services and Administration Managers |
| `sales_marketing_and_development_managers` | Sales, Marketing and Development Managers |
| `production_managers_in_agriculture_forestry_and_fisheries` | Production Managers in Agriculture, Forestry and Fisheries |
| `manufacturing_mining_construction_and_distribution_managers` | Manufacturing, Mining, Construction and Distribution Managers |
| `information_and_communications_technology_service_managers` | Information and Communications Technology Service managers |
| `professional_services_managers` | Professional Services Managers |
| `hotel_and_restaurant_managers` | Hotel and Restaurant Managers |
| `retail_and_wholesale_trade_managers` | Retail and Wholesale Trade Managers |
| `other_services_managers` | Other Services Managers |
| `physical_and_earth_science_professionals` | Physical and Earth Science Professionals |
| `mathematicians_actuaries_and_statisticians` | Mathematicians, Actuaries and Statisticians |
| `life_science_professionals` | Life Science Professionals |
| `engineering_professionals_excluding_electrotechnology` | Engineering Professionals (excluding Electrotechnology) |
| `electrotechnology_engineers` | Electrotechnology Engineers |
| `architects_planners_surveyors_and_designers` | Architects, Planners, Surveyors and Designers |
| `medical_doctors` | Medical Doctors |
| `nursing_and_midwifery_professionals` | Nursing and Midwifery Professionals |
| `traditional_and_complementary_medicine_professionals` | Traditional and Complementary Medicine Professionals |
| `paramedical_practitioners` | Paramedical Practitioners |
| `veterinarians` | Veterinarians |
| `other_health_professionals` | Other Health Professionals |
| `university_and_higher_education_teachers` | University and Higher Education Teachers |
| `vocational_education_teachers` | Vocational education teachers |
| `secondary_education_teachers` | Secondary Education Teachers |
| `primary_school_and_early_childhood_teachers` | Primary School and Early Childhood Teachers |
| `other_teaching_professionals` | Other Teaching Professionals |
| `finance_professionals` | Finance Professionals |
| `administration_professionals` | Administration Professionals |
| `sales_marketing_and_public_relations_professionals` | Sales, Marketing and Public Relations Professionals |
| `software_and_applications_developers_and_analysts` | Software and Applications Developers and Analysts |
| `database_and_network_professionals` | Database and Network Professionals |
| `legal_professionals` | Legal Professionals |
| `librarians_archivists_and_curators` | Librarians, Archivists and Curators |
| `social_and_religious_professionals` | Social and Religious Professionals |
| `authors_journalists_and_linguists` | Authors, Journalists and Linguists |
| `creative_and_performing_artists` | Creative and Performing Artists |
| `physical_and_engineering_science_technicians` | Physical and Engineering Science Technicians |
| `mining_manufacturing_and_construction_supervisors` | Mining, Manufacturing and Construction Supervisors |
| `process_control_technicians` | Process Control Technicians |
| `life_science_technicians_and_related_associate_professionals` | Life Science Technicians and Related Associate Professionals |
| `ship_and_aircraft_controllers_and_technicians` | Ship and Aircraft Controllers and Technicians |
| `medical_and_pharmaceutical_technicians` | Medical and Pharmaceutical Technicians |
| `nursing_and_midwifery_associate_professionals` | Nursing and Midwifery Associate Professionals |
| `traditional_and_complementary_medicine_associate_professionals` | Traditional and Complementary Medicine Associate Professionals |
| `veterinary_technicians_and_assistants` | Veterinary Technicians and Assistants |
| `other_health_associate_professionals` | Other Health Associate Professionals |
| `financial_and_mathematical_associate_professionals` | Financial and Mathematical Associate Professionals |
| `sales_and_purchasing_agents_and_brokers` | Sales and Purchasing Agents and Brokers |
| `business_services_agents` | Business Services Agents |
| `administrative_and_specialized_secretaries` | Administrative and Specialized Secretaries |
| `government_regulatory_associate_professionals` | Government Regulatory Associate Professionals |
| `legal_social_and_religious_associate_professionals` | Legal, Social and Religious Associate Professionals |
| `sports_and_fitness_workers` | Sports and Fitness Workers |
| `artistic_cultural_and_culinary_associate_professionals` | Artistic, Cultural and Culinary Associate Professionals |
| `information_and_communications_technology_operations_and_user_support_technicians` | Information and Communications Technology Operations and User Support Technicians |
| `telecommunications_and_broadcasting_technicians` | Telecommunications and Broadcasting Technicians |
| `general_office_clerks` | General Office Clerks |
| `secretaries_general` | Secretaries (general) |
| `keyboard_operators` | Keyboard Operators |
| `tellers_money_collectors_and_related_clerks` | Tellers, Money Collectors and Related Clerks |
| `client_information_workers` | Client Information Workers |
| `numerical_clerks` | Numerical Clerks |
| `material_recording_and_transport_clerks` | Material Recording and Transport Clerks |
| `other_clerical_support_workers` | Other Clerical Support Workers |
| `travel_attendants_conductors_and_guides` | Travel Attendants, Conductors and Guides |
| `cooks` | Cooks |
| `waiters_and_bartenders` | Waiters and Bartenders |
| `hairdressers_beauticians_and_related_workers` | Hairdressers, Beauticians and Related Workers |
| `building_and_housekeeping_supervisors` | Building and Housekeeping Supervisors |
| `other_personal_services_workers` | Other Personal Services Workers |
| `street_and_market_salespersons` | Street and Market Salespersons |
| `shop_aalespersons` | Shop Aalespersons |
| `cashiers_and_ticket_clerks` | Cashiers and Ticket Clerks |
| `other_sales_workers` | Other Sales Workers |
| `child_care_workers_and_teachers_aides` | Child Care Workers and Teachers’ Aides |
| `personal_care_workers_in_health_services` | Personal Care Workers in Health Services |
| `protective_services_workers` | Protective Services Workers |
| `market_gardeners_and_crop_growers` | Market Gardeners and Crop Growers |
| `animal_producers` | Animal Producers |
| `mixed_crop_and_animal_producers` | Mixed Crop and Animal Producers |
| `forestry_and_related_workers` | Forestry and Related Workers |
| `fishery_workers_hunters_and_trappers` | Fishery Workers, Hunters and Trappers |
| `subsistence_crop_farmers` | Subsistence Crop Farmers |
| `subsistence_livestock_farmers` | Subsistence Livestock Farmers |
| `subsistence_mixed_crop_and_livestock_farmers` | Subsistence Mixed Crop and Livestock Farmers |
| `subsistence_fishers_hunters_trappers_and_gatherers` | Subsistence Fishers, Hunters, Trappers and Gatherers |
| `building_frame_and_related_trades_workers` | Building Frame and Related Trades Workers |
| `building_finishers_and_related_trades_workers` | Building Finishers and Related Trades Workers |
| `painters_building_structure_cleaners_and_related_trades_workers` | Painters, Building Structure Cleaners and Related Trades Workers |
| `sheet_and_structural_metal_workers_moulders_and_welders_and_related_trades_workers` | Sheet and Structural Metal Workers, Moulders and Welders, and Related Trades Workers |
| `blacksmiths_toolmakers_and_related_trades_workers_trades_workers` | Blacksmiths, Toolmakers and Related Trades Workers (trades workers) |
| `machinery_mechanics_and_fitters` | Machinery Mechanics and Fitters |
| `electrical_equipment_installers_and_repairers_trades_workers` | Electrical Equipment Installers and Repairers (trades workers) |
| `electronics_and_telecommunications_installers_and_repairers_trades_workers` | Electronics and Telecommunications Installers and Repairers (trades workers) |
| `food_processing_woodworking_garment_and_other_craft_and_related_trades_workers` | Food Processing, Woodworking, Garment and Other Craft and Related Trades Workers |
| `mining_and_mineral_processing_plant_operators_plant_operators` | Mining and Mineral Processing Plant Operators (plant operators) |
| `metal_processing_and_finishing_plant_operators_plant_operators` | Metal Processing and Finishing Plant Operators (plant operators) |
| `chemical_and_photographic_products_plant_and_machine_operators_plant_operators` | Chemical and Photographic Products Plant and Machine Operators (plant operators) |
| `rubber_plastic_and_paper_products_machine_operators_plant_operators` | Rubber, Plastic and Paper Products Machine Operators (plant operators) |
| `textile_fur_and_leather_products_machine_operators_plant_operators` | Textile, Fur and Leather Products Machine Operators (plant operators) |
| `food_and_related_products_machine_operators_plant_operators` | Food and Related Products Machine Operators (plant operators) |
| `wood_processing_and_papermaking_plant_operators_plant_operators` | Wood Processing and Papermaking Plant Operators (plant operators) |
| `other_stationary_plant_and_machine_operators_plant_operators` | Other Stationary Plant and Machine Operators (plant operators) |
| `assemblers_plant_operators` | Assemblers (plant operators) |
| `locomotive_engine_drivers_and_related_workers_drivers` | Locomotive Engine Drivers and Related Workers (drivers) |
| `car_van_and_motorcycle_drivers_drivers` | Car, Van and Motorcycle Drivers (drivers) |
| `heavy_truck_and_bus_drivers_drivers` | Heavy Truck and Bus Drivers (drivers) |
| `mobile_plant_operators_drivers` | Mobile Plant Operators (drivers) |
| `ships_deck_crews_and_related_workers_drivers` | Ships’ Deck Crews and Related Workers (drivers) |
| `domestic_hotel_and_office_cleaners_and_helpers_labourers` | Domestic, Hotel and Office Cleaners and Helpers (labourers) |
| `vehicle_window_laundry_and_other_hand_cleaning_workers_labourers` | Vehicle, Window, Laundry and Other Hand Cleaning Workers (labourers) |
| `agricultural_forestry_and_fishery_labourers_labourers` | Agricultural, Forestry and Fishery Labourers (labourers) |
| `mining_and_construction_labourers_labourers` | Mining and Construction Labourers (labourers) |
| `manufacturing_labourers_labourers` | Manufacturing Labourers (labourers) |
| `transport_and_storage_labourers_labourers` | Transport and Storage Labourers (labourers) |
| `food_preparation_assistants_labourers` | Food Preparation Assistants (labourers) |
| `street_and_related_service_workers_labourers` | Street and Related Service Workers (labourers) |
| `street_and_market_salespersons_excluding_food` | Street and Market Salespersons (excluding food) |
| `garbage_collectors_and_other_elementary_workers` | Garbage Collectors and Other Elementary Workers |
| `commissioned_armed_forces_officers_armed_forces` | Commissioned Armed Forces Officers (armed forces) |
| `non_commissioned_armed_forces_officers_armed_forces` | Non-commissioned Armed Forces Officers (armed forces) |
| `armed_forces_occupations_other_ranks_armed_forces` | Armed Forces Occupations, Other Ranks (armed forces) |
| `unemployed` | Unemployed |

### 2. Job titles — `associated_persons[].position`

> 📘 Authoritative API Endpoint
> `GET /api/v1/job-titles`

Global list of job titles (9 options):

| `value` | label |
|---|---|
| `founder` | Founder |
| `chiefExecutiveOfficer` | Chief executive officer |
| `chiefComplianceOfficer` | Chief compliance officer |
| `chiefFinancialOfficer` | Chief financial officer |
| `chiefOperatingOfficer` | Chief operating officer |
| `president` | President |
| `vicePresident` | Vice president |
| `director` | Director |
| `other` | Other |

### 3. Company types — `company.type`

> 📘 Fixed Enum List
> Documented inline in the onboarding guides (not available from dynamic endpoints).

Fixed enum (10 options):

| `value` | label |
|---|---|
| `soleProprietorship` | Sole proprietorship |
| `partnerships` | Partnerships |
| `limitedLiabilityCompany` | Limited liability company |
| `corporation` | Corporation |
| `corporationPublic` | Corporation-public |
| `cooperative` | Cooperative |
| `nonProfitOrganization` | Non-Profit organization |
| `stateOwnedCompany` | State-owned company |
| `trust` | Trust |
| `other` | Other |

### 4. Source of funds — `company.source_of_funds`

> 📘 Fixed Enum List
> Documented inline in the onboarding guides (not available from dynamic endpoints).

Fixed enum (6 options):

| `value` | label |
|---|---|
| `BUSINESS_INCOME` | Business Income |
| `CAPITAL_INJECTION` | Capital Injection |
| `BANK_LOAN_OR_OTHER_BORROWINGS` | Bank Loan or Other Borrowings |
| `INVESTMENT_INCOME` | Investment Income |
| `GOVERNMENT_GRANT` | Government Grant |
| `OTHER` | Other Legitimate Source of Funds (Details Required) |

### 5. Identity document types — `id_type`, `identity_document.type`

> 📘 Fixed Enum List
> Enforced server-side. The valid set of document types depends on the issuing country.

Fixed enum: `NATIONAL_ID`, `PASSPORT`, `DRIVER_LICENCE`, `RESIDENCE_PERMIT`.

### 6. Supported countries

> 📘 Enforced Server-Side
> Enforced server-side; not available via lookup endpoints.

Used by `*.country`, `nationality`, `country_of_operation[]`, `tax_jurisdiction_country`. ISO 3166-1 alpha-2. The 224 recognized codes are:

`AD` `AE` `AG` `AI` `AL` `AM` `AQ` `AR` `AS` `AT` `AU` `AW` `AX` `AZ` `BA` `BB` `BD` `BE` `BF` `BG` `BH` `BJ` `BL` `BM` `BN` `BO` `BQ` `BR` `BS` `BT` `BV` `BW` `BZ` `CA` `CC` `CG` `CH` `CK` `CL` `CM` `CN` `CO` `CR` `CV` `CW` `CX` `CY` `CZ` `DE` `DJ` `DK` `DM` `DO` `DZ` `EC` `EE` `EG` `ER` `ES` `EU` `FI` `FJ` `FK` `FM` `FO` `FR` `FX` `GA` `GB` `GD` `GE` `GF` `GG` `GH` `GI` `GL` `GM` `GN` `GP` `GQ` `GR` `GS` `GT` `GU` `GY` `HK` `HM` `HN` `HR` `HT` `HU` `ID` `IE` `IL` `IM` `IN` `IO` `IS` `IT` `JE` `JM` `JO` `JP` `KE` `KG` `KH` `KI` `KM` `KN` `KR` `KW` `KY` `KY` `KZ` `LA` `LC` `LI` `LK` `LS` `LT` `LU` `LV` `MA` `MC` `MD` `ME` `MF` `MG` `MH` `MK` `MN` `MO` `MP` `MQ` `MR` `MS` `MT` `MU` `MV` `MW` `MX` `MY` `MZ` `NA` `NC` `NE` `NG` `NI` `NL` `NO` `NP` `NR` `NZ` `OM` `PA` `PE` `PF` `PG` `PH` `PK` `PL` `PM` `PN` `PR` `PS` `PT` `PW` `PY` `QA` `RE` `RO` `RS` `RW` `SA` `SB` `SC` `SE` `SG` `SH` `SI` `SJ` `SK` `SL` `SM` `SN` `SR` `ST` `SV` `SX` `SZ` `TC` `TD` `TF` `TG` `TH` `TJ` `TK` `TL` `TM` `TN` `TO` `TR` `TT` `TV` `TW` `TZ` `UG` `UM` `US` `UY` `UZ` `VA` `VC` `VG` `VI` `VN` `VU` `WF` `WS` `XK` `YT` `ZA` `ZM` `ZZ`

### 7. Prohibited countries

> 📘 Enforced Server-Side
> Enforced server-side; not available via lookup endpoints.

Prohibited countries (20 codes):

`CU` `IR` `KP` `AF` `BY` `CD` `CF` `GW` `IQ` `LY` `ML` `MM` `RU` `SD` `SO` `SS` `SY` `UA` `YE` `ZW`

### 8. Address subdivisions — example: US subdivisions

> 📘 Authoritative API Endpoint
> `GET /api/v1/countries/{country}/subdivisions`
> Enforced for country codes: `US`, `CA`, `AU`, `CN`, `HK`.

United States (57), as an example:

| `code` | name |
|---|---|
| `AK` | Alaska |
| `AL` | Alabama |
| `AR` | Arkansas |
| `AZ` | Arizona |
| `CA` | California |
| `CO` | Colorado |
| `CT` | Connecticut |
| `DC` | District of Columbia |
| `DE` | Delaware |
| `FL` | Florida |
| `GA` | Georgia |
| `HI` | Hawaii |
| `IA` | Iowa |
| `ID` | Idaho |
| `IL` | Illinois |
| `IN` | Indiana |
| `KS` | Kansas |
| `KY` | Kentucky |
| `LA` | Louisiana |
| `MA` | Massachusetts |
| `MD` | Maryland |
| `ME` | Maine |
| `MI` | Michigan |
| `MN` | Minnesota |
| `MO` | Missouri |
| `MS` | Mississippi |
| `MT` | Montana |
| `NC` | North Carolina |
| `ND` | North Dakota |
| `NE` | Nebraska |
| `NH` | New Hampshire |
| `NJ` | New Jersey |
| `NM` | New Mexico |
| `NV` | Nevada |
| `NY` | New York |
| `OH` | Ohio |
| `OK` | Oklahoma |
| `OR` | Oregon |
| `PA` | Pennsylvania |
| `RI` | Rhode Island |
| `SC` | South Carolina |
| `SD` | South Dakota |
| `TN` | Tennessee |
| `TX` | Texas |
| `UT` | Utah |
| `VA` | Virginia |
| `VT` | Vermont |
| `WA` | Washington |
| `WI` | Wisconsin |
| `WV` | West Virginia |
| `WY` | Wyoming |
| `AS` | American Samoa |
| `GU` | Guam |
| `MP` | Northern Mariana Islands |
| `PR` | Puerto Rico |
| `UM` | United States Minor Outlying Islands |
| `VI` | U.S. Virgin Islands |

### 9. Industries — example: US industries

> 📘 Authoritative API Endpoint
> `GET /api/v1/countries/{country}/industries`
> (e.g. `GET /api/v1/countries/US/industries`)

Short US excerpt to show the shape:

| `value` | label | (section) |
|---|---|---|
| `soybean_farming` | Soybean Farming | Agriculture, Forestry, Fishing and Hunting |
| `oilseed_except_soybean_farming` | Oilseed (except Soybean) Farming | Agriculture, Forestry, Fishing and Hunting |
| `dry_pea_and_bean_farming` | Dry Pea and Bean Farming | Agriculture, Forestry, Fishing and Hunting |
| `wheat_farming` | Wheat Farming | Agriculture, Forestry, Fishing and Hunting |
| `crude_petroleum_extraction` | Crude Petroleum Extraction | Mining |
| `natural_gas_extraction` | Natural Gas Extraction | Mining |
