---
title: What is XQ-XML
---
## What is XQ-XML

XQ-XML is the XML file format for setting up questions used in exams, revisions excercises, practice tests and Quizes. This includes different formats of questions (e.g. Multiple choice, true-or-false, fill in the gap, short answers, longer text answers etc) and different formats of question sets (e.g. quick tests, mixed question type test, timed and untimed tests etc). It also covers various subjects e.g. Maths, English, Geography etc.

This document will set out the format and usage of the XML file with examples and use cases.

The user needs a some knowledge of XML. A good tutorial will be found **here** https://www.w3schools.com/xml/. The important thing is that XML is very easy to understand, that is why it is used extensively in I.T. for handling and sharing data between different applications and within various systems. HTML is a form of XML, so if you know some HTML you're already on your way.  

In this document, we will present all the elements of XQ-XML and demonstrate how you can write valid XQ-XML files. We will aslo show how you can check whether your XQ-XML file is valid before going on to use them with the excercise, test or quiz App of your choice. 

Like all XML files, XQ-XML consists of specific tags. The top level tag is `<QuestionSets></QuestionSets>`. This tag is used to specify a set of tests/excercises/exams each one in a `<QuestionSet></QuestionSet>` tag. Each of the tests/excercises/exams may then contain various questions `<Question></Question>`.

See example of this below.
```
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
```
This is a very simple example for a very simple type test and question type. Look below to see how the tests are rendered in a Quiz Application.

<div id="questionSetContainer" class="ui card" style="width:100%;height:70%;">
  <span id="selectContainer"></span>
  <p/>
  <div id="questionContainer">
  </div>
</div>


<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/semantic-ui@2.5.0/dist/semantic.min.css">
<script src="https://code.jquery.com/jquery-3.6.1.min.js" integrity="sha256-o88AwQnZB+VDvE9tvIXrMQaPlFFSUTR+nldQm1LuPXQ=" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/semantic-ui@2.5.0/dist/semantic.min.js"></script>


<script>
    $('.ui.dropdown').dropdown();
</script>

<script>
var currentQuestionIdx=0;
var currentQuestionSolution;
var currentNumberOfQuestionsInSet=3;

renderQuestionsets("https://raw.githubusercontent.com/SamuelAina/XQ-XML-DOCS/main/data/Three%20Tests%20for%20my%20students.xml");

function renderXQXML(myxml){
  parser = new DOMParser();
  xmlDoc = parser.parseFromString(myxml,"text/xml");
  questionSets =xmlDoc.getElementsByTagName("QuestionSet");
  
  selectContainerHtml=`
  <select class="ui search dropdown"  id="questionSetSelid" onchange="selectTest()">
      <option>(none)</option>
	  ${Array.from(questionSets)
	         .map(function(e){return `<option>${e.getAttribute("title")}</option>`})
			 .join("")
	   }
  </select>
  `
  document.getElementById("selectContainer").innerHTML=selectContainerHtml;
  
  $('.ui.dropdown').dropdown();
  $("#selectContainer > div").css("width","100%");
}


function selectTest(){
    document.getElementById("questionContainer").innerHTML="";
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
 
function displayCurrentQuestion(){
 		currentQuestion=selectedQuestionSet.getElementsByTagName("Question")[currentQuestionIdx];
		currentNumberOfQuestionsInSet=selectedQuestionSet.getElementsByTagName("Question").length;
		currentQuestionSolution=currentQuestion.querySelector('solution_text').innerHTML;
		currentQuestionHTML=`
			<br/>
			<span class="ui ignored warning message"><b>Question Number ${currentQuestionIdx+1}</b></span>
			<span class="ui info message">${currentQuestion.querySelector('question_text').innerHTML}</span>
			<br/><br/><br/>
			<div class="ui container">
			<label class="ui horizontal huge label">Your answer:</label>
			<div class="ui input" >
			<input id="userAnswer" type="text" placeholder="type your answer here..."></input>
			</div>
			<button class="ui large primary button" onclick="checkAnswer()">Check Answer</button><br/><br/><br/>
			<button class="ui large primary button" onclick="prevQuestion()" >Prev</button>
			<button class="ui large primary button" onclick="nextQuestion()" style="margin-left:23%;">Next</button>
			</div>			
		`;
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
 
function renderQuestionsets(url){
    var request = new XMLHttpRequest();
    request.open('GET', url, true);
    request.send(null);
    request.onreadystatechange = function () {
        if (request.readyState === 4 && request.status === 200) {
		    myxml=request.responseText;
			renderXQXML(myxml)
		    return 
        }
    }
}


</script>
