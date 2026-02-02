# MA 
- https://www.kff.org/medicare/how-medicare-pays-medicare-advantage-plans-issues-and-policy-options/#06bea8de-754d-491c-aad8-9e8f1df31977
# NPPES NPI
- https://download.cms.gov/nppes/NPI_Files.html  

# New_Enrollee_Job_Aid cameraready 011413.pdf 
- https://www.csscoperations.com/internet/csscw3_files.nsf/F/CSSCNew_Enrollee_Job_Aid%20cameraready%20011413.pdf/$FILE/New_Enrollee_Job_Aid%20cameraready%20011413.pdf

# MOR record type
- https://www.cms.gov/files/document/informationontheimplementationofthe2025rxhccriskadjustmentmodelsandnormalizationfactorsg.pdf
- L: Model version V24. 2023 ESRD (Dialysis and Functioning Graft) 
- J: Model version V24 - 2020 CMS-HCC  
- M: Model version V28 - 2024 CMS-HCC  

# Provider type - specialty - taxonomy  
- https://www.pa.gov/content/dam/copapwp-pagov/en/dhs/documents/providers/documents/npi/NPI-Taxonomy-Crosswalk.pdf
- 
# CMS CCN HOSPITAL - Hospital General Information 
- https://data.cms.gov/provider-data/dataset/xubh-q36u

# CMS Quality net star ratings data + IQR etc. 
- https://qualitynet.cms.gov/inpatient/public-reporting/overall-ratings/data-collection

# H-Num + Plan beneficiay package (PBP) number: SNP Type, Organization Name. Contract number.
- https://www.cms.gov/data-research/statistics-trends-and-reports/medicare-advantagepart-d-contract-and-enrollment-data/monthly-enrollment-contract/plan/state/county
- special needs plan (snp): CSNP, DSNP, ISNP, NonSNP
- https://www.cms.gov/data-research/statistics-trends-and-reports/medicare-advantagepart-d-contract-and-enrollment-data/special-needs-plan-snp-data
- 
# new enrollee
-  https://www.csscoperations.com/internet/csscw3_files.nsf/F/CSSCNew_Enrollee_Job_Aid%20cameraready%20011413.pdf/$FILE/New_Enrollee_Job_Aid%20cameraready%20011413.pdf
- new enrollee: risk_adjustment_factor_type_code: (E, ED, E1, E2, SE in field 47 of MMR file)  

- https://1library.net/article/mmr-flat-file-layout-continued-service-claims-component.q0j7pl3z   
```
# 47 RA Factor Type Code 2 189-190
Type of factors in use (see Fields 24-25):
C = Community
C1 = Community Post-Graft I (ESRD)
C2 = Community Post-Graft II (ESRD)
D = Dialysis (ESRD)

E = New Enrollee
ED = New Enrollee Dialysis (ESRD)
E1 = New Enrollee Post-Graft I (ESRD)
E2 = New Enrollee Post-Graft II (ESRD)
G1 = Graft I (ESRD)
G2 = Graft II (ESRD) I = Institutional
I1 = Institutional Post-Graft I (ESRD)
I2 = Institutional Post-Graft II (ESRD)

# 48 Frailty Indicator 1 191-191
Y = MCO-level Frailty Factor Included

# 49 Original Reason for Entitlement Code (OREC) 1 192-192
0 = Beneficiary insured due to age
1 = Beneficiary insured due to disability
2 = Beneficiary insured due to ESRD
3 = Beneficiary insured due to disability and current ESRD
```

# H-Num
- Ref: https://www.cms.gov/data-research/statistics-trends-and-reports/medicare-advantagepart-d-contract-and-enrollment-data/ma-plan-directory
- Ref: https://www.cms.gov/data-research/statistics-trends-and-reports/medicare-advantagepart-d-contract-and-enrollment-data/ma-plan-directory
- Medicare Advantage Contract Number  
- https://resdac.org/cms-data/variables/medicare-part-c-contract-number
  - the contract number = H_num
  - plan benefit package number = PB_Num  

# https://resdac.org/cms-data/variables/beneficiary-race-code-encounter  

https://qpp.cms.gov/  

[2022 Traditional MIPS Scoring Guide](https://qpp-cm-prod-content.s3.amazonaws.com/uploads/1970/2022%20Traditional%20MIPS%20Scoring%20Guide.pdf)  
- HCC contribution in the MIPS payment adjust factor   

https://www.cms.gov/files/document/cclf-information-packet.pdf   
https://www.cms.gov/Medicare/Medicare-Fee-for-Service-Payment/sharedsavingsprogram/program-guidance-and-specifications   

https://qualitynet.cms.gov/   
https://qualitynet.cms.gov/inpatient/measures/readmission/methodology   


# CCLF  
- https://www.cms.gov/medicare/medicare-fee-for-service-payment/sharedsavingsprogram/program-guidance-and-specifications  
- [Claim and Claim Line Feed Information Packet v36 (PDF)](https://www.cms.gov/media/540186)  
- version 40 - https://www.cms.gov/files/document/cclf-information-packet.pdf  

# RVU  
- https://www.cms.gov/Medicare/Medicare-Fee-for-Service-Payment/PhysicianFeeSched/PFS-Relative-Value-Files    
- -https://www.aapc.com/tools/rvu-calculator.aspx   
- https://www.cms.gov/medicare/payment/fee-schedules/physician/pfs-relative-value-files  
  - official CPT - RVU mapping  
  - Geographic Practice Cost Indices (GPCIs)   
- https://www.cms.gov/medicare/payment/fee-schedules/physician-fee-schedule/locality-key  
  - locality-key lookup: Medicare administrative contractor (MAC) and Locality Number  
- https://www.aapc.com/resources/what-are-relative-value-units-rvus 
  - Total RVUs  
  - Geographic Practice Cost Indices (GPCIs)  
  - Conversion Factor (CF)

# https://www.ncqa.org/wp-content/uploads/2023-0706_Digital-Measurement-Midyear-Review-DISPLAY.pdf

# Loinc codes  
- https://loinc.org/downloads/

- # HCC Risk adjustment factor
- https://www.cms.gov/medicare/health-plans/medicareadvtgspecratestats/risk-adjustors-items/cpt-hcpcs
- 
