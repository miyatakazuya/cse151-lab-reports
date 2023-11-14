# Lab Report 3

## Part 1 - Bugs

**Failure-inducing Input**  
```
import static org.junit.Assert.*;
import org.junit.*;

public class ArrayTests {
  @Test 
	public void testReverseInPlace() {
    int[] input2 = {1, 2, 3, 4, 5};
    ArrayExamples.reverseInPlace(input2);
    assertArrayEquals(new int[]{ 5, 4, 3, 2, 1 }, input2);
	}
}
```

**Output:**  
```
JUnit version 4.13.2
.E
Time: 0.01
There was 1 failure:
1) testReverseInPlace(ArrayTests)
arrays first differed at element [2]; expected:<1> but was:<3>
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:78)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:28)
        at org.junit.Assert.internalArrayEquals(Assert.java:534)
        at org.junit.Assert.assertArrayEquals(Assert.java:418)
        at org.junit.Assert.assertArrayEquals(Assert.java:429)
        at ArrayTests.testReverseInPlace(ArrayTests.java:11)
        ... 30 trimmed
Caused by: java.lang.AssertionError: expected:<1> but was:<3>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at org.junit.internal.ExactComparisonCriteria.assertElementsEqual(ExactComparisonCriteria.java:8)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:76)
        ... 36 more

FAILURES!!!
Tests run: 1,  Failures: 1
```

**Input that doesn't induce a failure**  
```
import static org.junit.Assert.*;
import org.junit.*;

public class ArrayTests {
  @Test 
	public void testReverseInPlace() {
    int[] input2 = {1};
    ArrayExamples.reverseInPlace(input2);
    assertArrayEquals(new int[]{ 1 }, input2);
	}
}
```  
**Output:**  
```
JUnit version 4.13.2
.
Time: 0.007

OK (1 test)
```

**Program (Before Fix)**  
```
public class ArrayExamples {

  // Changes the input array to be in reversed order
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
  
}
```

**Program (After Fix)**  
```
public class ArrayExamples {

  // Changes the input array to be in reversed order
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < (arr.length / 2); i += 1) {
      int temp = arr[i]; // Temporary Varible stores overriden value
      arr[i] = arr[arr.length - i - 1]; 
      arr[arr.length - i - 1] = temp;
    }
  }

}
```

* Analysis: The original method fails to pass the failure-inducing input because when overriding the value of [0] in the array, the value overriden is not actually switched with the other value. So in the failure inducing input above, when the index of [2] overrides [0], the contents of the input become  [3, 2, 3]. The method terminates here, meaning the test fails because the value at [2] is never properly switched out.

## Part 2 - Researching Commands (Command: `Find`)
The basic usage of the command `find` is:   
* find <path>
but there are several other specific command-line tools that can be used to perform more specific tasks.

### 1. **-name** 
Source: https://linux.die.net/man/1/find
* Narrows the search results to files with the specified name  

Usage 1:  
```
kamiyata@LAPTOP-39212087:/mnt/c/Users/kazuy/School/cse15L/docsearch/technical$ find biomed/ -name "*213*"
biomed/1471-213X-1-1.txt
biomed/1471-213X-1-10.txt
biomed/1471-213X-1-11.txt
biomed/1471-213X-1-12.txt
biomed/1471-213X-1-13.txt
biomed/1471-213X-1-15.txt
biomed/1471-213X-1-2.txt
biomed/1471-213X-1-3.txt
biomed/1471-213X-1-4.txt
biomed/1471-213X-1-6.txt
biomed/1471-213X-1-9.txt
biomed/1471-213X-2-1.txt
biomed/1471-213X-2-7.txt
biomed/1471-213X-2-8.txt
biomed/1471-213X-3-2.txt
biomed/1471-213X-3-3.txt
biomed/1471-213X-3-4.txt
biomed/1471-213X-3-7.txt
```
> This is helpful in this case because the -name command-line is utilized to find all files in /biomed/ that contains the string "213", demonstrating its usefulness in finding files with specific names  

Usage 2:  
```
kamiyata@LAPTOP-39212087:/mnt/c/Users/kazuy/School/cse15L/docsearch/technical$ find government/About_LSC -name "*txt"
government/About_LSC/Comments_on_semiannual.txt
government/About_LSC/commission_report.txt
government/About_LSC/conference_highlights.txt
government/About_LSC/CONFIG_STANDARDS.txt
government/About_LSC/diversity_priorities.txt
government/About_LSC/LegalServCorp_v_VelazquezDissent.txt
government/About_LSC/LegalServCorp_v_VelazquezOpinion.txt
government/About_LSC/LegalServCorp_v_VelazquezSyllabus.txt
government/About_LSC/ODonnell_et_al_v_LSCdecision.txt
government/About_LSC/ONTARIO_LEGAL_AID_SERIES.txt
government/About_LSC/Progress_report.txt
government/About_LSC/Protocol_Regarding_Access.txt
government/About_LSC/reporting_system.txt
government/About_LSC/Special_report_to_congress.txt
government/About_LSC/State_Planning_Report.txt
government/About_LSC/State_Planning_Special_Report.txt
government/About_LSC/Strategic_report.txt
```
> This demonstrates another way to use -name. By using the * operator, the command-line can be used to locate files with names that end with a specific filetype like txt.

### 2. **-size** <n>[c]  
Source: https://linux.die.net/man/1/find  
* Narrows down the search to files within a certain parameter based off file-size. You must specifcy a unit with 'c' representing bytes, 'k' for kilobytes, 'M' for megabytes, and 'G' for gigabytes. The + or - operator should be added to specify if the results should be less than or greater than the speficied file size.  
  
Usage 1:  
```
kamiyata@LAPTOP-39212087:/mnt/c/Users/kazuy/School/cse15L/docsearch/technical$ find biomed -size +75k
biomed/1471-2105-3-18.txt
biomed/1471-2105-3-2.txt
biomed/1471-2202-3-1.txt
biomed/1472-6882-1-10.txt
biomed/1472-6904-2-5.txt
biomed/1476-511X-1-2.txt
biomed/gb-2002-3-12-research0083.txt
biomed/gb-2002-3-12-research0086.txt
```  
> This represents a useful example of -size in which it used to find files that are greater than 75 Kilobyytes within biomed.

Usage 2:
```
kamiyata@LAPTOP-39212087:/mnt/c/Users/kazuy/School/cse15L/docsearch/technical$ find biomed -size -10k
biomed
biomed/1471-2334-3-13.txt
biomed/1471-2490-3-2.txt
```
> In this case the - operator is used to find files within biomed that are less than 10 Kilobytes.  \

### 3. -type <type>  
Source: https://linux.die.net/man/1/find  
* Used to filter results by filetype (Example 'd' for directories and 'f' for files)  

Usage 1:
```
kamiyata@LAPTOP-39212087:/mnt/c/Users/kazuy/School/cse15L/docsearch/technical$ find government -type d
government
government/About_LSC
government/Alcohol_Problems
government/Env_Prot_Agen
government/Gen_Account_Office
government/Media
government/Post_Rate_Comm
```
> Without the -type command line, the find result would contain every single .txt file under all of the sub-directories in /government/ . By specifying that the filetype should be 'd' for directories, the output is narrowed down to the directories within the specified path. This makes it a useful way to find what directores exist under a path.  

Usage 2:
```
kamiyata@LAPTOP-39212087:/mnt/c/Users/kazuy/School/cse15L/docsearch/technical$ find government/Alcohol_Problems/ -type f
government/Alcohol_Problems/DraftRecom-PDF.txt
government/Alcohol_Problems/Session2-PDF.txt
government/Alcohol_Problems/Session3-PDF.txt
government/Alcohol_Problems/Session4-PDF.txt
```
> Similarly to the other usage, this limits the output to only actual files which eliminates the directories under the specified path ./government/Alchohool_Problems which means that is not returned along with the find results.  

### 4. -maxdepth / -mindepth 	
Source: https://linux.die.net/man/1/find  
* Used to specifcy the directory depth at which the search occurs.

Usage 1:
```
kamiyata@LAPTOP-39212087:/mnt/c/Users/kazuy/School/cse15L/docsearch/technical$ find government/ -maxdepth 1
government/
government/About_LSC
government/Alcohol_Problems
government/Env_Prot_Agen
government/Gen_Account_Office
government/Media
government/Post_Rate_Comm
```
> By specifying the max depth of the search as 1, the results only show the directories that lie in the first level of government/.

Usage 2:
```
kamiyata@LAPTOP-39212087:/mnt/c/Users/kazuy/School/cse15L/docsearch/technical$ find government -mindepth 2
government/About_LSC/Comments_on_semiannual.txt
government/About_LSC/commission_report.txt
government/About_LSC/conference_highlights.txt
government/About_LSC/CONFIG_STANDARDS.txt
government/About_LSC/diversity_priorities.txt
government/About_LSC/LegalServCorp_v_VelazquezDissent.txt
government/About_LSC/LegalServCorp_v_VelazquezOpinion.txt
government/About_LSC/LegalServCorp_v_VelazquezSyllabus.txt
government/About_LSC/ODonnell_et_al_v_LSCdecision.txt
government/About_LSC/ONTARIO_LEGAL_AID_SERIES.txt
government/About_LSC/Progress_report.txt
government/About_LSC/Protocol_Regarding_Access.txt
government/About_LSC/reporting_system.txt
government/About_LSC/Special_report_to_congress.txt
government/About_LSC/State_Planning_Report.txt
government/About_LSC/State_Planning_Special_Report.txt
government/About_LSC/Strategic_report.txt
government/Alcohol_Problems/DraftRecom-PDF.txt
government/Alcohol_Problems/Session2-PDF.txt
government/Alcohol_Problems/Session3-PDF.txt
government/Alcohol_Problems/Session4-PDF.txt
government/Env_Prot_Agen/1-3_meth_901.txt
government/Env_Prot_Agen/atx1-6.txt
government/Env_Prot_Agen/bill.txt
government/Env_Prot_Agen/ctf1-6.txt
government/Env_Prot_Agen/ctf7-10.txt
government/Env_Prot_Agen/ctm4-10.txt
government/Env_Prot_Agen/final.txt
government/Env_Prot_Agen/jeffordslieberm.txt
government/Env_Prot_Agen/multi102902.txt
government/Env_Prot_Agen/nov1.txt
government/Env_Prot_Agen/ro_clear_skies_book.txt
government/Env_Prot_Agen/section-by-section_summary.txt
government/Env_Prot_Agen/tech_adden.txt
government/Env_Prot_Agen/tech_sectiong.txt
government/Gen_Account_Office/ai00134.txt
government/Gen_Account_Office/ai2132.txt
government/Gen_Account_Office/ai9868.txt
government/Gen_Account_Office/d01121g.txt
government/Gen_Account_Office/d01145g.txt
government/Gen_Account_Office/d01186g.txt
government/Gen_Account_Office/d01376g.txt
government/Gen_Account_Office/d01591sp.txt
government/Gen_Account_Office/d0269g.txt
government/Gen_Account_Office/d02701.txt
government/Gen_Account_Office/d03232sp.txt
government/Gen_Account_Office/d03273g.txt
government/Gen_Account_Office/d03419sp.txt
government/Gen_Account_Office/ffm.txt
government/Gen_Account_Office/gg96118.txt
government/Gen_Account_Office/GovernmentAuditingStandards_yb2002ed.txt
government/Gen_Account_Office/im814.txt
government/Gen_Account_Office/InternalControl_ai00021p.txt
government/Gen_Account_Office/July11-2001_gg00172r.txt
government/Gen_Account_Office/June30-2000_gg00135r.txt
government/Gen_Account_Office/Letter_Walkeraug17let.txt
government/Gen_Account_Office/Letter_WalkerJan30-2001.txt
government/Gen_Account_Office/May1998_ai98068.txt
government/Gen_Account_Office/Oct15-1999_gg00026t.txt
government/Gen_Account_Office/Oct15-2001_d0224.txt
government/Gen_Account_Office/og96009.txt
government/Gen_Account_Office/og96011.txt
government/Gen_Account_Office/og96012.txt
government/Gen_Account_Office/og96014.txt
government/Gen_Account_Office/og96015.txt
government/Gen_Account_Office/og96020.txt
government/Gen_Account_Office/og96021.txt
government/Gen_Account_Office/og96022.txt
government/Gen_Account_Office/og96023.txt
government/Gen_Account_Office/og96026.txt
government/Gen_Account_Office/og96027.txt
government/Gen_Account_Office/og96028.txt
government/Gen_Account_Office/og96031.txt
government/Gen_Account_Office/og96032.txt
government/Gen_Account_Office/og96033.txt
government/Gen_Account_Office/og96034.txt
government/Gen_Account_Office/og96036.txt
government/Gen_Account_Office/og96037.txt
government/Gen_Account_Office/og96038.txt
government/Gen_Account_Office/og96040.txt
government/Gen_Account_Office/og96041.txt
government/Gen_Account_Office/og96042.txt
government/Gen_Account_Office/og96043.txt
government/Gen_Account_Office/og96045.txt
government/Gen_Account_Office/og96047.txt
government/Gen_Account_Office/og97001.txt
government/Gen_Account_Office/og97002.txt
government/Gen_Account_Office/og97003.txt
government/Gen_Account_Office/og97011.txt
government/Gen_Account_Office/og97019.txt
government/Gen_Account_Office/og97020.txt
government/Gen_Account_Office/og97023.txt
government/Gen_Account_Office/og97028.txt
government/Gen_Account_Office/og97032.txt
government/Gen_Account_Office/og97038.txt
government/Gen_Account_Office/og97039.txt
government/Gen_Account_Office/og97041.txt
government/Gen_Account_Office/og97043.txt
government/Gen_Account_Office/og97045.txt
government/Gen_Account_Office/og97046.txt
government/Gen_Account_Office/og97050.txt
government/Gen_Account_Office/og97051.txt
government/Gen_Account_Office/og97052.txt
government/Gen_Account_Office/og98018.txt
government/Gen_Account_Office/og98019.txt
government/Gen_Account_Office/og98022.txt
government/Gen_Account_Office/og98024.txt
government/Gen_Account_Office/og98026.txt
government/Gen_Account_Office/og98029.txt
government/Gen_Account_Office/og98030.txt
government/Gen_Account_Office/og98032.txt
government/Gen_Account_Office/og98040.txt
government/Gen_Account_Office/og98041.txt
government/Gen_Account_Office/og98044.txt
government/Gen_Account_Office/og98045.txt
government/Gen_Account_Office/og98046.txt
government/Gen_Account_Office/og99036.txt
government/Gen_Account_Office/Paper_Walker11-2002_acpro122.txt
government/Gen_Account_Office/pe1019.txt
government/Gen_Account_Office/Sept14-2002_d011070.txt
government/Gen_Account_Office/Sept27-2002_d02966.txt
government/Gen_Account_Office/Statements_Feb28-1997_volume.txt
government/Gen_Account_Office/Testimony_cg00010t.txt
government/Gen_Account_Office/Testimony_d01609t.txt
government/Gen_Account_Office/Testimony_Jul15-2002_d02940t.txt
government/Gen_Account_Office/Testimony_Jul17-2002_d02957t.txt
government/Media/5_Legal_Groups.txt
government/Media/Abuse_penalties.txt
government/Media/Advocate_for_Poor.txt
government/Media/agency_expands.txt
government/Media/Aid_Gets_7_Million.txt
government/Media/All_May_Have_Justice.txt
government/Media/Annual_Fee.txt
government/Media/Anthem_Payout.txt
government/Media/AP_LawSchoolDebts.txt
government/Media/Assuring_Underprivileged.txt
government/Media/Attorney_gives_his_time.txt
government/Media/Avoids_Budget_Cut.txt
government/Media/A_helping_hand.txt
government/Media/A_Perk_of_Age.txt
government/Media/balance_scales_of_justice.txt
government/Media/Barnes_new_job.txt
government/Media/Barnes_pro_bono.txt
government/Media/Barnes_Volunteers.txt
government/Media/Barr_sharpening_ax.txt
government/Media/BergenCountyRecord.txt
government/Media/Bias_on_the_Job.txt
government/Media/Boone_legal_service.txt
government/Media/Bridging_legal_aid_gap.txt
government/Media/BusinessWire.txt
government/Media/BusinessWire2.txt
government/Media/Butler_Co_attorneys.txt
government/Media/Campaign_Pays.txt
government/Media/City_Council_Budget.txt
government/Media/Civil_Matters.txt
government/Media/CommercialAppealMemphis2.txt
government/Media/Commercial_Appeal.txt
government/Media/Coup_Reshapes_Legal_Aid.txt
government/Media/Court_Keeps_Judge_From.txt
government/Media/Crains_New_York_Business.txt
government/Media/defend_yourself.txt
government/Media/Disaster_center.txt
government/Media/Do-it-yourself_divorce.txt
government/Media/Domestic_violence_aid.txt
government/Media/Domestic_Violence_Ruling.txt
government/Media/Donald_Hilliker.txt
government/Media/Entities_Merge.txt
government/Media/Eviction_law.txt
government/Media/families_saved.txt
government/Media/Farm_workers.txt
government/Media/Federal_agency.txt
government/Media/Few_who_need.txt
government/Media/fight_domestic_abuse.txt
government/Media/Fire_Victims_Sue.txt
government/Media/Firm_to_the_Poor_Needs_Help.txt
government/Media/FortWorthStarTelegram.txt
government/Media/Free_Legal_Assistance.txt
government/Media/Free_legal_service.txt
government/Media/Funding_cuts_force.txt
government/Media/Funding_May_Limit.txt
government/Media/Funds_Shortage.txt
government/Media/FY_04_Budget_Outlook.txt
government/Media/Ginny_Kilgore.txt
government/Media/Good_guys_reward.txt
government/Media/grants_fail_to_come.txt
government/Media/Greedy_Generous.txt
government/Media/GreensburgDailyNews.txt
government/Media/Hard_to_Get.txt
government/Media/Helping_Hands.txt
government/Media/Helping_Out.txt
government/Media/help_rent-to-own_tenants.txt
government/Media/Higher_court.txt
government/Media/Higher_Registration_Fees.txt
government/Media/highlight_Senior_Day.txt
government/Media/IOLTA_INTEREST_RATE.txt
government/Media/It_Pays_to_Know.txt
government/Media/Justice_for_all.txt
government/Media/Justice_requests.txt
government/Media/Kiosks_for_court_forms.txt
government/Media/Law-school_grads.txt
government/Media/Lawyer_Web_Survey.txt
government/Media/Law_Award_from_College.txt
government/Media/Law_Schools.txt
government/Media/Legal-aid_chief.txt
government/Media/Legal_Aid_attorney.txt
government/Media/Legal_Aid_campaign.txt
government/Media/Legal_Aid_in_Clay_County.txt
government/Media/Legal_Aid_looks_to_legislators.txt
government/Media/Legal_Aid_Society.txt
government/Media/Legal_hotline.txt
government/Media/Legal_services_for_poor.txt
government/Media/Legal_system_fails_poor.txt
government/Media/less_legal_aid.txt
government/Media/Library_Lawyers.txt
government/Media/Lindsays_legacy.txt
government/Media/Local_Attorneys.txt
government/Media/Lockyer_Warns.txt
government/Media/Low-income_children.txt
government/Media/Major_Changes.txt
government/Media/Making_a_case.txt
government/Media/man_on_national_team.txt
government/Media/Marylands_Legal_Aid.txt
government/Media/New_funding_sources.txt
government/Media/New_Online_Resources.txt
government/Media/NJ_Legal_Services.txt
government/Media/Nonprofit_Buys.txt
government/Media/not_accessible_to_disabled.txt
government/Media/Oregon_Poor.txt
government/Media/Owning_a_Piece.txt
government/Media/Paralegal_Honored.txt
government/Media/Philly_Lawyers.txt
government/Media/Politician_Practices.txt
government/Media/Poor_Lacking_Legal_Aid.txt
government/Media/Poverty_Lawyers.txt
government/Media/predatory_loans.txt
government/Media/Pro-bono_road_show.txt
government/Media/Program_Lodges.txt
government/Media/Providing_Legal_Aid.txt
government/Media/pro_bono_efforts.txt
government/Media/Pro_Bono_Services.txt
government/Media/Raising_the_Bar.txt
government/Media/Rental_rules.txt
government/Media/residents_sue_city.txt
government/Media/Retirement_Has_Its_Appeal.txt
government/Media/RoanokeTimes.txt
government/Media/Rumble_in_the_Bronx.txt
government/Media/Self-Help_Website.txt
government/Media/Service_Agency.txt
government/Media/State_funding.txt
government/Media/Supporting_Legal_Center.txt
government/Media/Survey.txt
government/Media/Targeting_Domestic_Violence.txt
government/Media/Terrorist_Attack.txt
government/Media/Texas_Lawyer.txt
government/Media/Texas_Supreme_Court.txt
government/Media/The_Bend_Bulletin.txt
government/Media/The_Columbian.txt
government/Media/The_State_of_Pro_Bono.txt
government/Media/Too_Crucial_to_Take_Cut.txt
government/Media/Towson_Attorney.txt
government/Media/Understanding.txt
government/Media/Unusual_Woodburn.txt
government/Media/Using_Tech_Tools.txt
government/Media/Valley_Needing_Legal_Services.txt
government/Media/Volunteers_Step_Up.txt
government/Media/water_fees.txt
government/Media/Weak_economy.txt
government/Media/Wilmington_lawyer.txt
government/Media/Wingates_winds.txt
government/Media/Workers_aid_center.txt
government/Media/Working_for_Free.txt
government/Post_Rate_Comm/Cohenetal_comparison.txt
government/Post_Rate_Comm/Cohenetal_Cost_Function.txt
government/Post_Rate_Comm/Cohenetal_CreamSkimming.txt
government/Post_Rate_Comm/Cohenetal_DeliveryCost.txt
government/Post_Rate_Comm/Cohenetal_RuralDelivery.txt
government/Post_Rate_Comm/Cohenetal_Scale.txt
government/Post_Rate_Comm/Gleiman_EMASpeech.txt
government/Post_Rate_Comm/Gleiman_gca2000.txt
government/Post_Rate_Comm/Mitchell_6-17-Mit.txt
government/Post_Rate_Comm/Mitchell_RMVancouver.txt
government/Post_Rate_Comm/Mitchell_spyros-first-class.txt
government/Post_Rate_Comm/Redacted_Study.txt
government/Post_Rate_Comm/ReportToCongress2002WEB.txt
government/Post_Rate_Comm/WolakSpeech_usps.txt
```
> This represents an case in which the search is specified files than is at least 2 directories deep from the specified directory which excludes the direcotries that exist at the first level of government/.

