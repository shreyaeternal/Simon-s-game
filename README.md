<!DOCTYPE html>
<html lang="en" dir="ltr">

<head>
  <meta charset="utf-8">
  <title>Simon</title>
  <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Press+Start+2P">
  <style>
    body {
      text-align: center;
      background-color: #011F3F;
    }

    #level-title {
      font-family: 'Press Start 2P', cursive;
      font-size: 3rem;
      margin: 5%;
      color: #FEF2BF;
    }

    .container {
      display: block;
      width: 50%;
      margin: auto;
    }

    .btn {
      margin: 25px;
      display: inline-block;
      height: 200px;
      width: 200px;
      border: 10px solid black;
      border-radius: 20%;
    }

    .game-over {
      background-color: red;
      opacity: 0.8;
    }

    .red {
      background-color: red;
    }

    .green {
      background-color: green;
    }

    .blue {
      background-color: blue;
    }

    .yellow {
      background-color: yellow;
    }

    .pressed {
      box-shadow: 0 0 20px white;
      background-color: grey;
    }
  </style>
</head>

<body>
  <h1 id="level-title">Press A Key to Start</h1>
  <div class="container">
    <div class="row">
      <div type="button" id="green" class="btn green"></div>
      <div type="button" id="red" class="btn red"></div>
    </div>
    <div class="row">
      <div type="button" id="yellow" class="btn yellow"></div>
      <div type="button" id="blue" class="btn blue"></div>
    </div>
  </div>
  <script src="https://code.jquery.com/jquery-1.12.4.min.js" integrity="sha256-ZosEbRLbNQzLpnKIkEdrPv7lOy9C27hHQ+Xp8a4MxAQ=" crossorigin="anonymous"></script>
  <script>
    var buttonColours = ["red", "blue", "green", "yellow"];
    var gamePattern = [];
    var userClickedPattern = [];
    var started = false;
    var level = 0;

    $(document).keypress(function () {
      if (!started) {
        $("#level-title").text("Level " + level);
        nextSequence();
        started = true;
      }
    });

    $(".btn").click(function () {
      var userChosenColour = $(this).attr("id");
      userClickedPattern.push(userChosenColour);
      playSound(userChosenColour);
      animatePress(userChosenColour);
      checkAnswer(userClickedPattern.length - 1);
    });

    function checkAnswer(currentLevel) {
      if (gamePattern[currentLevel] === userClickedPattern[currentLevel]) {
        console.log("success");

        if (userClickedPattern.length === gamePattern.length) {
          setTimeout(function () {
            nextSequence();
          }, 1000);
        }
      } else {
        console.log("wrong");
        playSound("wrong");

        $("body").addClass("game-over");
        setTimeout(function () {
          $("body").removeClass("game-over");
        }, 200);

        $("#level-title").text("Game Over, Press Any Key to Restart");
        startOver();
      }
    }

    function startOver() {
      level = 0;
      gamePattern = [];
      started = false;
    }

    function nextSequence() {
      userClickedPattern = [];
      level++;
      $("#level-title").text("Level " + level);

      var randomNumber = Math.floor(Math.random() * 4);
      var randomChosenColour = buttonColours[randomNumber];
      gamePattern.push(randomChosenColour);

      $("#" + randomChosenColour).fadeIn(100).fadeOut(100).fadeIn(100);
      playSound(randomChosenColour);
    }

    function playSound(name) {
      var audio = new Audio("sounds/" + name + ".mp3");
      audio.play();
    }

    function animatePress(currentColor) {
      $("#" + currentColor).addClass("pressed");
      setTimeout(function () {
        $("#" + currentColor).removeClass("pressed");
      }, 100);
    }
  </script>
</body>

</html>

