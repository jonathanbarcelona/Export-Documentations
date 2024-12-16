COUPA PROJECT NAME: BH_PHOTO

Project ID: 73

EXPORT/OUTPUT SPECIFICATION:

LST input file:

LST Header:

COUNTER|IMAGETIF|TYPE|INVOICE NUMBER|VENDOR ID|VENDOR NAME|INVOICE DATE|SALES TAX|FREIGHT|SUBMITFORAPPROVAL|ATTENTIONTO|EMAILADDRESS|ACCOUNT-CODE|VENDORADDRESS|PONUMBER|CHARTOFACCOUNTS|CURRENCY|REQUIREDPO|INVOICETOTAL|INVOICECOMPUTED|ASSIGNED|LINE_LEVEL_MATCH

LST Line-Item:

COUNTER|IMAGETIF|TYPE|INVOICE NUMBER|VENDOR ID|VENDOR NAME|LINE NUMBER|DESCRIPTION|SUPPLIERPART|PRICE|QUANTITY|UOM|PONUMBER|POLINENUMBER|ACCOUNT-CODE

No Tax Line is required for BH_PHOTO.

CSV Output:

BH_PHOTO captures both headers and line item. The CSV output format headers and line items are as follows.

1.  Invoice,Invoice Number*,Supplier Name,Supplier Number,Status,Invoice Date*,Submit For Approval?,Tax Amount,Shipping Amount,Misc Amount,Line Level Taxation*,Chart of Accounts*,Currency,Requester Email,Image Scan Filename,ap_assigned_user,Supplier Note

2.  Invoice Line,Invoice Number*,Supplier Name,Supplier Number,Line Number,Description*,Supplier Part Number,Price*,Quantity,Unit of Measure*,PO Number,PO Line Number,Account Code,Account Segment 1,Account Segment 2,Account Segment 3,Account Segment 4

The item #1 of the CSV Output uses the value found in the LST Header of the input file.

The item #2 of the CSV output uses the value found in the LST Line-Item of the input file.

Pre-checking requirements:

1.  Check the number of pipe-separator if it matches the number of fields defined in the LST header.

2.  If difference between the INVOICETOTAL vs INVOICECOMPUTED is more than $10000, check the invoice.

Below are the requirements for the headers and line item.

Headers:

1.  Compute if there is a difference with INVOICETOTAL vs INVOICECOMPUTED. Difference is to be included in the MISC Amount in the CSV.

2.  Credit Note is not applicable for BH_PHOTO

3.  If InvoiceNumber = "" Then SubmitForApproval = "NO"

4.  If VendorName = "" Then SubmitForApproval = "NO"

5.  If InvoiceDate = "" Then SubmitForApproval = "NO"

6.  If ChartOfAccount = "" Then SubmitForApproval = "NO"

7.  If Currency = "" Then SubmitForApproval = "NO"

8.  REQUIREDPO field is either Y or N. (Not TRUE or FALSE)

9.  If Misc = 0, Misc = “”

10. For Supplier Notes

    a.  If PO Number is captured and [REQUIRED PO/PO FOUND CSV] = FALSE/N then Supplier Notes = PO# & [PurchaseOrder] & " not in the PO File." / Set Approval = NO

    b.  If PO Number is captured and [REQUIRED PO/PO FOUND CSV] = TRUE/Y and one of the PO LINE NUMBER is missing, then Supplier Notes = "PO# " & [PurchaseOrder] & " captured. PO-Line not matching." / Set Approval = NO

    c.  If Vendor Number is missing then Supplier Notes = Missing Supplier Name/ID

    d.  If Invoice Number is missing then Supplier Notes = Invoice Number Missing

11. If Invoice Number = "???" then mark the document as NON-INVOICE. Do not upload file to Coupa. Delivered only the PDF file to the client using the email address assigned to it.

12. If AccountCode <> “”, split AccountCode + “----” by “-”. Value 1 should go AccountSegment1, value 2 should go to AccountSegment2, value 3 should go to AccountSegment3, value 4 should go to AccountSegment4 in this order.

13. If AccountSegment1 = “” and AccountSegment2 = “” and AccountSegment3 = “” and AccountSegment4, AccountCode=””

14. If Line_Level_Match = “Y”, SubmitForApproval = NO

15. With CTI Portal output

Line Item:

1.  If Description is blank then set Description = "*" if REQUIRED PO/PO FOUND = FALSE

2.  If Description is blank then set Description = [PurchaseOrder] & " captured. PO-Line not matching." if REQUIRED PO/PO FOUND = TRUE

3.  If the PO Number in the Line-Item Level is blank, make sure that the PO Line Number is set to blank.

4.  If the PO NUMBER has value + PO LINE NUMBER has value, do not populate value Account Code, Account Segment 1, Account Segment 2, Account Segment 3, Account Segment 4

5.  If the PO LINE NUMBER is blank, use the value in the Account Code to populate the Account Segment 1, Account Segment 2, Account Segment 3 and Account Segment 4. You will need to split the value in the Account code using "-", and place them in the Account Segment 1, Account Segment 2, Account Segment 3 and Account Segment 4 in this order. Do not populate the Account Code field in the CSV output.

6.  If AccountSegment1 = “” and AccountSegment2 = “” and AccountSegment3 = “” and AccountSegment4

Non-Coupa Requirements:

1.  Create an equivalent CTI Portal output that will be uploaded to https://ctiwebportal.com/user/login/projects

2.  Historical/Duplicate: NO

3.  Daily Summary: NO

4.  Non-Invoice: NO
