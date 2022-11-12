---
title: What is XQ-XML
---
## What is XQ-XML

XQ-XML is the XML file format for setting up questions used in Exams, revisions, practice, Tests and Quizes. This includes different formats of questions (e.g. Multiple choice, true-or-false, fill in the gap, short answers, longer text answers etc) and different formats of question sets (e.g. quick tests, mixed question type test, timed and untimed tests etc). It also covers various subjects e.g. Maths, English, Geography etc.

This document will set out the format and usage of the XML file with examples and use cases.

The user needs a some knowledge of XML and DTD. A good tutorial will be found **here**. The important thing is that XML is very easy to understand, that is why it is used extensively in IT as a means of handling and sharing data among different applications and systems. HTML is a form of XML, so if you know some HTML you're already on your way.

In this document, we will present all the elements of XQ-XML and demonstrate how you can write valid XQ-XML files. We will aslo show how you can check whether your XQ-XML file is valid before going on to use them with the Excercise, Test or Quiz Application of your choice. 

Like all XML files, XQ-XML consists of specific tags. The top level tag is <QuestionSets></QuestionSets>. This tag is used to specify a set of tests/excercise/exams each one in a <QuestionSet></QuestionSet> tag. Each of the tests/excercise/exams may then contain various questions <Question></Question>.

See example of this below.

<QuestionSets title="Three Tests for my students">
   <QuestionSet title="Test 1">
      <Question>
	      <question_text>What id the capital of the United Kingdom?</question_text>
		  <solution_text>London</solution_text>
	  </Question>
      <Question>
	      <question_text>How many colours are in the rainbow?</question_text>
		  <solution_text>7</solution_text>
	  </Question>
      <Question>
	      <question_text></question_text>
		  <solution_text></solution_text>
	  </Question>	  
   </QuestionSet> 
   <QuestionSet title="Test 2">
      <Question>
	      <question_text></question_text>
		  <solution_text></solution_text>
	  </Question>
      <Question>
	      <question_text></question_text>
		  <solution_text></solution_text>
	  </Question>
      <Question>
	      <question_text></question_text>
		  <solution_text></solution_text>
	  </Question>	  
   </QuestionSet>  
   <QuestionSet title="Test 3">
      <Question>
	      <question_text></question_text>
		  <solution_text></solution_text>
	  </Question>
      <Question>
	      <question_text></question_text>
		  <solution_text></solution_text>
	  </Question>
      <Question>
	      <question_text></question_text>
		  <solution_text></solution_text>
	  </Question>	  
   </QuestionSet>    
</QuestionSets>

This is a very simple example for a very simple type test and question type. Use the link below to see how the tests are rendered in a Quiz Application.