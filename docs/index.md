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
```
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
```
This is a very simple example for a very simple type test and question type. Look below to see how the tests are rendered in a Quiz Application.

<div id="questionSetContainer" style="border:thin;height:10%;width:90%;background-color:lightblue;margin:15px;">
  <span>Test:</span>
  <span id="selectContainer">
  <select id="questionSetSelid" onchange="selectTest()">
      <option>(none)</option>
	  <option>Test 1</option><option>Test 2</option><option>Test 3</option>
  </select>
  </span>
  <p>
  </p><div id="questionContainer">
		</p></div>
</div>
<script>
var myxml=`
 <QuestionSets title="Three Tests for my students">
   <QuestionSet title="Test 1">
      <Question>
	      <question_text>What is the capital of the United Kingdom?</question_text>
		  <solution_text>London</solution_text>
	  </Question>
      <Question>
	      <question_text>How many colours are in the rainbow?</question_text>
		  <solution_text>7</solution_text>
	  </Question>
      <Question>
	      <question_text>How many loaves of bread are in a bakers dozen</question_text>
		  <solution_text>13</solution_text>
	  </Question>	  
   </QuestionSet> 
   <QuestionSet title="Test 2">
      <Question>
	      <question_text>Which animal has the longest neck?</question_text>
		  <solution_text>Giraffe</solution_text>
	  </Question>
      <Question>
	      <question_text>Which game is played by 11 players on both sides?</question_text>
		  <solution_text>Football</solution_text>
	  </Question>
      <Question>
	      <question_text>What is the square root of 64?</question_text>
		  <solution_text>8</solution_text>
	  </Question>	  
   </QuestionSet>  
   <QuestionSet title="Test 3">
      <Question>
	      <question_text>What is solid water called?</question_text>
		  <solution_text>ice</solution_text>
	  </Question>
      <Question>
	      <question_text>How may moths are in a year?</question_text>
		  <solution_text>12</solution_text>
	  </Question>
      <Question>
	      <question_text>How many pieces are in a chess game?</question_text>
		  <solution_text>32</solution_text>
	  </Question>	  
   </QuestionSet>    
</QuestionSets>   
    `
  parser = new DOMParser();
  xmlDoc = parser.parseFromString(myxml,"text/xml");
  questionSets =xmlDoc.getElementsByTagName("QuestionSet");
  
  selectContainerHtml=`
  <select id="questionSetSelid" onchange="selectTest()">
      <option>(none)</option>
	  ${Array.from(questionSets)
	         .map(function(e){return `<option>${e.getAttribute("title")}</option>`})
			 .join("")
	   }
  </select>
  `
  document.getElementById("selectContainer").innerHTML=selectContainerHtml;

 var currentQuestionIdx=0;
 var currentQuestionSolution;
 function selectTest(){
    currentQuestionIdx=0;
	selectedValue=document.getElementById("questionSetSelid").value;   
	selectedQuestionSet = xmlDoc.querySelector('QuestionSet[title="'+selectedValue+'"]')
	if(selectedQuestionSet){
		displayCurrentQuestion();
	}
 } 
 
 function checkAnswer(){
    userAnswer = document.getElementById("userAnswer").value;
	if(userAnswer==""){
	  alert("Please provide an answer");
	  return;
	}
	if(userAnswer.toUpperCase() == currentQuestionSolution.toUpperCase()){
	  alert("CORRECT!!");
	}else{
	  alert(userAnswer + " is WRONG!!");	
    }	
 }
 
 var currentNumberOfQuestionsInSet=3;
 function displayCurrentQuestion(){
 		currentQuestion=selectedQuestionSet.getElementsByTagName("Question")[currentQuestionIdx];
		currentNumberOfQuestionsInSet=selectedQuestionSet.getElementsByTagName("Question").length;
		currentQuestionSolution=currentQuestion.querySelector('solution_text').innerHTML;
		currentQuestionHTML=`
			<span>Question Number ${currentQuestionIdx+1} of ${currentNumberOfQuestionsInSet}</span>
			<h3>${currentQuestion.querySelector('question_text').innerHTML}</h3>
			<span>type your answer here:</span><input id="userAnswer" type="text"></input>
			<button onclick="checkAnswer()">Check Answer</button><p/>
			<button onclick="prevQuestion()" style="margin-left:2%;">Prev</button><button  onclick="nextQuestion()" style="margin-left:25%;">Next</button>			
		`;
		console.log(currentQuestionHTML);
		document.getElementById("questionContainer").innerHTML=currentQuestionHTML;    
 }
 
 function nextQuestion(){
   if(currentQuestionIdx<currentNumberOfQuestionsInSet-1){
     currentQuestionIdx=currentQuestionIdx+1;
	 displayCurrentQuestion();
   }
 }
 
 function prevQuestion(){
    if(currentQuestionIdx>0){
     currentQuestionIdx=currentQuestionIdx-1;
	 displayCurrentQuestion();
   }
 }
</script>
