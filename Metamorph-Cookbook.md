***Use recursion to avoid the value of a field to be reset***  
4 lines of text should be extracted from a pica+ record which all begin with the $f-subfield of the 003@  and are followed by a combination of subfields in each 028@.   
003@ ƒ01025201213  
028@ ƒdFarīdƒcal-ƒaʿAbdalāwī  
028@ ƒdFarīdƒaAl-ʿAbdalāwī  
028@ ƒdFarid HamidƒaEl Abdellaouiƒ4nawi  
028@ ƒdFarid HamidƒaAbdellaouiƒlEl  

The rule of metamorph is defined as:  
'&lt;combine name="${personalname}${surname}${forename}${prefix}${addition}" 
 value="##${pid}##,##${personalname}${surname}${forename}${prefix}${addition}##,##V##" 
 flushWith="028@" reset="true"&gt;
&lt;data source="003@.0" name="pid"/>
&lt;data source="028@.P" name="personalname" />
&lt;data source="028@.a" name="surname"/>
&lt;data source="028@.d" name="forename"/>
&lt;data source="028@.c" name="prefix"/>
&lt;concat name="addition" delimiter=", " flushWith="028A" reset="true" prefix=" ">
&lt;data source="028@.n"/>
&lt;data source="028@.l"/>
&lt;data source="028@.g"/>
&lt;/concat>
&lt;/combine>'

The following output is expected:  
'##1025201213##,##Abdellaoui, Farid Hamid##,##V##'  
'##1025201213##,##Al-ʿAbdalāwī, Farīd##,##V##'  
'##1025201213##,##El Abdellaoui, Farid Hamid##,##V##'  
'##1025201213##,##ʿAbdalāwī, Farīd al-##,##V##'  

In fact output looks like this:  
'##1025201213##,##Abdellaoui, Farid Hamid##,##V##'  
'####,##Al-ʿAbdalāwī, Farīd##,##V##'  
'####,##El Abdellaoui, Farid Hamid##,##V##'  
'####,##ʿAbdalāwī, Farīd al-##,##V##'  
The reset=”true” option cleans the 003@ and 028@ before a new 028@ is read. A new 003@ doesn’t exist and it is therefore empty. Neither reset=”false” can be used here, because it will remember the value of the old 028@, like the “al-“ at end of the following lines which in fact only appears in the first 028@ field.
'##1025201213##,##Abdellaoui, Farid Hamid al-##,##V##'  
'##1025201213##,##Al-ʿAbdalāwī, Farīd al-##,##V##'  
'##1025201213##,##El Abdellaoui, Farid Hamid al-##,##V##'  
'##1025201213##,##ʿAbdalāwī, Farīd al-##,##V##'  

The solution is use of recursion (prefix @):

<combine name="@pid" value="${pid}" flushWith="028@" >
	<data source="003@.0" name="pid"/>
</combine>
   	
<combine name="${personalname}${surname}${forename}${prefix}${addition}" 
	value="##${pid}##,##${personalname}${surname}${forename}${prefix}${addition}##,##V##"
		flushWith="028@" reset="true">
	<data source="@pid" name="pid"/>
	<data source="028@.P" name="personalname" />
   	<data source="028@.a" name="surname"/>
	<data source="028@.d" name="forename"/>
	<data source="028@.c" name="prefix"/>
	<concat name="addition" delimiter=", " flushWith="028A" reset="true" prefix=" ">
		<data source="028@.n"/>
		<data source="028@.l"/>
		<data source="028@.g"/>
	</concat>
</combine>

The first <combine> element use the name “@pid” to declare a redirecting the value return to the input stream and it is done for each new 028@. The second <combine> element uses the redirected, but not reset value of 003@ to combine the new 028@ value which are reset. 

