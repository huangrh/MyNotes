### https://www.mmitnetwork.com/what-are-hcpcs-codes/  
```
https://hcpcs.codes/j-codes/
```

```
The Healthcare Common Procedure Coding System (HCPCS) is a collection of standardized codes used in medical billing to represent various medical procedures, services, products and supplies in claims to Medicare, Medicaid, and many third-party payers.

HCPCS is divided into Level I and Level II.

Level I consists of Current Procedural Terminology (CPT-4) codes, which are unique identification numbers (and accompanying descriptions) for all medical services and procedures provided by healthcare professionals. These codes are published annually by the American Medical Association.

Level II HCPCS codes identify products, supplies, and services that are not included in the CPT codes, such as ambulance services, durable medical equipment, and medication administered in a physician’s office. Most pharmaceutical products managed under the medical benefit have HCPCS codes. These codes are published quarterly by CMS.
What are the types of HCPCS Level II codes?

All HCPCS Level II codes are five characters in length, beginning with a letter and followed by four digits. The letter (A through V) represents the code chapter. Some of the most common HCPCS Level II code types, or chapters, are:

E-codes: Used to report all durable medical equipment
G-codes: Used to report temporary procedures and professional services
H-codes: Used to report rehabilitation services
J-codes: Used to report all non-orally administered prescription medications and chemotherapy drugs
```

### CMS Risk Adjustable CPT
https://www.cms.gov/medicare/health-plans/medicareadvtgspecratestats/risk-adjustors-items/cpt-hcpcs 

### Specialty code 
- 2025
https://catalog.data.gov/dataset/medicare-provider-and-supplier-taxonomy-crosswalk-368b9
- old 
https://www.cms.gov/Medicare/Provider-Enrollment-and-Certification/MedicareProviderSupEnroll/Downloads/taxonomy.pdf

### Chronic Care Management (CCM) and CPT Codes: 99490, 99439, 99487, 99489, 99491
- https://timedochealth.com/2021/04/23/chronic-care-management-ccm-and-cpt-codes-99490-99439-99487-99489-99491/

- The national average for each CCM CPT code is as followed:  
```
CCM CPT Code: 99490: $42.84  
CCM CPT Code: 99491: $74.26  
CCM CPT Code: 99439: $38.00  
CCM CPT Code: 99487: $94.68  
CCM CPT Code: 99489: $45.00  
```

- CCM CPT codes 99487 and 99489 are often seen together and fall under complex CCM care.
- CCM CPT codes 99490 and 99439 are often seen together and are standard CCM care.
- CCM CPT codes 99491 and 99439 are seen in conjunction and cover standard CCM care.


### Office visit CPT/HCPCS Codes  
  IF(CLM_LINE_HCPCS_CD 
         RLIKE '99202|99203|99204|99205|99212|99213|99214|99215|99341|99342|99343|99344|99345|99347|99348|99349|99350|99386|99387|99396|99397|G0438|G0439'
         , 1, 0) office_visit_flag

### ACO38  
- https://nqfappservicesstorage.blob.core.windows.net/proddocs/30/Fall/2020/measures/2888/shared/2888.zip
### place of service code (POS)
https://www.cms.gov/Medicare/Medicare-Fee-for-Service-Payment/PhysicianFeeSched/Downloads/Website-POS-database.pdf  
https://www.cms.gov/Medicare/Coding/place-of-service-codes/Place_of_Service_Code_Set  

### Provider NPI    
- NPI has org_name, and address.     
	- https://data.cms.gov/provider-data/search?theme=Doctors%20and%20clinicians    
- NPPES Full Replacement Monthly NPI File    
	- https://download.cms.gov/nppes/NPI_Files.html      
	- https://download.cms.gov/nppes/NPPES_Data_Dissemination_February_2023.zip      

### nucc taxonomy code 
- https://www.cms.gov/Medicare/Provider-Enrollment-and-Certification/MedicareProviderSupEnroll/Downloads/TaxonomyCrosswalk.pdf  
- https://www.nucc.org/index.php/code-sets-mainmenu-41/provider-taxonomy-mainmenu-40/csv-mainmenu-57  

###  Transitional Care Management services - Face to face Visit  
- https://www.cms.gov/Outreach-and-Education/Medicare-Learning-Network-MLN/MLNProducts/Downloads/Transitional-Care-Management-Services-Fact-Sheet-ICN908628.pdf
- see page 7 

### Medicare Advantage Prescription Drug Star Ratings
- https://www.cms.gov/Medicare/Prescription-Drug-Coverage/PrescriptionDrugCovGenIn/PerformanceData

### Medicare Claim Process Mannual    
- https://www.cms.gov/Regulations-and-Guidance/Guidance/Manuals/Internet-Only-Manuals-IOMs-Items/CMS018912

### Mental Health Code
- https://www.cms.gov/files/document/mln1986542-medicare-mental-health.pdf
- 
###  Part B Drugs:   
- https://www.cms.gov/Medicare/Medicare-Fee-for-Service-Part-B-Drugs/McrPartBDrugAvgSalesPrice  
	- https://www.cms.gov/files/zip/october-2022-asp-ndc-hcpcs-crosswalks.zip 
- https://www.fda.gov/drugs/drug-approvals-and-databases/national-drug-code-directory  
	- https://www.cms.gov/Medicare/Fraud-and-Abuse/PhysicianSelfReferral/index  
- NDC: National Drug Code. 

[CCWD - Chronic Condition Categories](https://www2.ccwdata.org/web/guest/condition-categories-chronic)    
- [30 CCW Chronic Conditions (2017 forward) in pdf](https://www2.ccwdata.org/documents/10280/19139421/chr-chronic-condition-algorithms.pdf)

[HCUP-Chronic Condition Indicator](https://www.hcup-us.ahrq.gov/toolssoftware/chronic_icd10/chronic_icd10.jsp#download)  
- Chronic, Acute, Not Applicable, Both Chronic and Acute. [download link](https://www.hcup-us.ahrq.gov/toolssoftware/chronic_icd10/CCI-ICD10CM-v2021-1.zip)  

[CMS List of CPT/HCPCS Codes](https://www.cms.gov/Medicare/Fraud-and-Abuse/PhysicianSelfReferral?msclkid=915e55c3d13011ecb39f45d53e297bbe)  
[CPT Code Category-I, II, III ](https://en.wikipedia.org/wiki/Current_Procedural_Terminology)  
[CMS HCPCS QUARTERLY UPDATE-all HCPCS Level II updates](https://www.cms.gov/medicare/coding/hcpcsreleasecodesets/hcpcs-quarterly-update)  
[Overview of Coding and Classification Systems](https://www.cms.gov/cms-guide-medical-technology-companies-and-other-interested-parties/coding/overview-coding-classification-systems)

ICD-10 | CMS
•	2022 Code Descriptions in Tabular Order - Updated 02/01/2022 (ZIP)  
•	2022 ICD-10-PCS Codes File - updated December 1, 2021 (ZIP)

When medical coders and billers talk about HCPCS codes, they’re actually referring to HCPCS Level II codes.

Release	             Maintainer	Number of Codes	discharges				Release Frequency
2022 ICD-10-CM codes	CMS	72621		October 1, 2021 - September 30, 2022	Yearly in December
2022 ICD-10-PCS codes	CMS	78227		
2021 ICD-10-CM codes	CMS	72750		October 1, 2020 - September 30, 2021	
2021 ICD-10-PCS codes	CMS	78138		
HCPCS Level-1 (CPT-4th Revision – Category I)	American Medical Association (AMA)	>10,000		
HCPCS Level-2	CMS	7545 in Release 2022		Quarterly
				

CPT Codes:
Maintained by AMA
Released in Oct. every year
There are three categories: 
Category 1: Codes for evaluation and management (E/M codes), codes for anesthesia, codes for surgery, codes for radiology, codes for pathology and laboratory, and codes for medicine.
CPT Category I is the largest body of codes consisting of those commonly used by providers to report their services and procedures. 
Category 2: The codes are named in a pattern that begins with four digits followed by a capital letter.
CPT Category II consists of supplemental tracking codes used for performance management. 
Category 3: consists of temporary codes used to report emerging and experimental services and procedures. 

HCPCS Codes
•	Level I codes consist of the AMA's CPT codes and is numeric.
o	Level I HCPCS (CPT-4 codes) for hospital providers
o	Level I of the HCPCS is comprised of Current Procedural Terminology (CPT-4) , a numeric coding system maintained by the American Medical Association (AMA).
o	The CPT-4 is a uniform coding system consisting of descriptive terms and identifying codes that are used primarily to identify medical services and procedures furnished by physicians and other health care professionals. 
o	Level I of the HCPCS, the CPT-4 codes, does not include codes needed to separately report medical items or services that are regularly billed by suppliers other than physicians
•	Level II codes are the HCPCS alphanumeric code set and primarily include non-physician products, supplies, and procedures not included in CPT.
o	HCPCS Quarterly Update | CMS
o	HCPCS - General Information | CMS
The CPT codes associated with the routine medical services of Evaluation and Management (i.e., clinic visits), are also called E&M codes

 
-	
References:
1.	HCPCS Codes - HCPCS Level II Coding - AAPC
2.	NCCI Policy Manual for Medicare | CMS
3.	Introduction to HCPCS Level I Coding | Medical Billing and Coding U: All CPT codes are HCPCS codes, but not all HCPCS codes are CPT codes
4.	Revenue Codes - JF Part A - Noridian (noridianmedicare.com)
5.	List of Revenue Codes for Medical Billing (2022) - Medical Billing RCM
6.	UB04 Revenue Codes (findacode.com)
7.	Place of Service Code Set | CMS
8.	HCPCS Codes used for Whole blood - TOS "0"
9.	Professional Paper Claim Form (CMS-1500) | CMS
10.	The UB-04 (CMS-1450) form is the claim form for institutional facilities such as hospitals or outpatient facilities. This would include things like surgery, radiology, laboratory, or other facility services. The HCFA-1500 form (CMS-1500) is used to submit charges covered under Medicare Part B. If you work in a medical clinic, hospital, rehabilitation center or nursing home, then you would use the UB-04 claim form for billing purposes. If you are a physician or doctor, then you should fill out the CMS-1500 claim form to complete your billing. [source]
11.	

The purpose of revenue codes is to provide extra information about a procedure or service, such as where a patient received care. 
Revenue codes are on every billing line and are always linked with other codes used by healthcare professionals:
1.	ICD, diagnostic codes for each patient
2.	CPT, procedure codes for each service given
3.	HCPCS, codes for products, supplies and services administered

https://resdac.org/cms-data/variables/revenue-center-code-ffs
The Transformed Medicaid Statistical Information System (T-MSIS) Analytic Files (TAF)
https://resdac.org/cms-data/variables/revenue-center-code-taf 
National Uniform Claim Committee - Provider Taxonomy (nucc.org)
CSV is where you can download a Comma Separated Values (CSV) version of the code set. 
