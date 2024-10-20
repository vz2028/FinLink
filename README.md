# SEC API Filings Automation  

## Overview  

This project automates the **retrieval of financial filings** from the SEC's EDGAR database using the `sec-api` library. It allows users to search for **key filings (8-K, 10-Q, and 10-K)** based on specific keywords and download recent reports from public companies directly into structured formats like **Excel and DataFrames**. The automation saves time by eliminating the need to manually search the SEC website for filings, which is useful for auditing, compliance, and financial research.  

---

## Why This Project is Useful  

- **Streamlines Financial Research**: Automates the search and retrieval of filings, saving time for auditors, analysts, and researchers.  
- **Customizable Searches**: Users can filter filings by **keywords, form types, and companies**, making it easy to focus on relevant data.  
- **Financial Report Downloads**: Directly provides URLs to Excel versions of financial reports, simplifying analysis.  
- **Consistent Reporting**: Extracts recent filings for specific companies, ensuring **up-to-date information** for reporting.  

---

## How to Get Started  

1. **Prerequisites**:  
   - Ensure you have **Python 3.7+** installed.  
   - Install the required libraries:  
     ```bash  
     pip install sec-api pandas  
     ```  

2. **Set Up the API Key**:  
   - Obtain your **SEC API key** from [sec-api.io](https://sec-api.io/).  
   - Replace the placeholder API key in the code with your own:  
     ```python  
     api_key = "your_api_key_here"  
     ```  

3. **Run the Code**:  
   - Execute the script to:  
     - Search for filings based on keywords like **“internal controls.”**  
     - Retrieve the **four most recent 10-K filings** for a company (e.g., Visa Inc.).  

4. **Expected Output**:  
   - A printed list of relevant filings matching your search criteria.  
   - A pandas DataFrame displaying details (e.g., company name, form type, filing date) with links to **Excel reports**.  

---

## Example Usage  

**Search for filings by keyword**:  
```python  
from sec_api import FullTextSearchApi  

FullTextSearchApi = FullTextSearchApi(api_key="your_api_key_here")  

query = {  
    "query": '"internal controls" OR "United States of America"',  
    "formTypes": ['8-K', '10-Q', '10-K'],  
    "page": 1  
}  
filings = FullTextSearchApi.get_filings(query)  
for filed in filings["filings"][4:10]:  
    print(filed)
```

**Fetch Visa’s recent 10-K filings**:
```python 
import pandas as pd  
from sec_api import QueryApi  

queryApi = QueryApi(api_key='your_api_key_here')  

query = {  
    "query": {"query_string": {  
        "query": "ticker: V AND formType:\"10-K\"",  
        "default_operator": "AND"  
    }},  
    "from": "0",  
    "size": "4",  
    "sort": [{"filedAt": {"order": "desc"}}]  
}  

response = queryApi.get_filings(query)  
data = [  
    {  
        "Company Name": filing["companyNameLong"],  
        "CIK": filing["cik"],  
        "Accession Number": filing["accessionNo"],  
        "Date Filed": filing["filedAt"],  
        "Form Type": filing["formType"],  
        "Financial Report URL": f"https://www.sec.gov/Archives/edgar/data/{filing['cik']}/{filing['accessionNo'].replace('-','')}/Financial_Report.xlsx"  
    }  
    for filing in response['filings']  
]  

outputs = pd.DataFrame(data)  
print(outputs.head(4))  
```

Where to Get Help
API Documentation: Refer to sec-api.io/docs for in-depth guidance.
Issues: If you encounter bugs or need help, create an issue on this repository’s Issues tab on GitHub.
Community Support: Check forums such as Stack Overflow for related discussions.
Contributing and Maintenance
This project is maintained by Vicky Zhao.

License
This project is available under the MIT License. Feel free to use and modify it as needed.

This project offers a fast, reliable way to fetch and analyze financial filings using the SEC API, helping users stay up-to-date on public company reports and supporting auditing, compliance, and financial research tasks.

