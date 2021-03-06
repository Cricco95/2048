# Smart-2048
A small clone of [1024](https://play.google.com/store/apps/details?id=com.veewo.a1024), based on [Saming's 2048](http://saming.fr/p/2048/) (also a clone) in which I implemented an artificial intelligence agent which solves the puzzle game.

Made just for fun.

The official app can also be found on the [Play Store](https://play.google.com/store/apps/details?id=com.gabrielecirulli.app2048) and [App Store!](https://itunes.apple.com/us/app/2048-by-gabriele-cirulli/id868076805)

# Minimax algorithm
The Minimax is a recursive algorithm which can be used for solving two-player zero-sum games. In each state of the game we associate a value. The Minimax algorithm searches through the space of possible game states creating a tree which is expanded until it reaches a particular predefined depth. Once those leaf states are reached, their values are used to estimate the ones of the intermediate nodes. This is an example of a tree generated by a Minimax algorithm:

![](http://blog.datumbox.com/wp-content/uploads/2014/04/Minimax.png)

The interesting idea of this algorithm is that each level represents the turn of one of the two players. In order to win each player must select the move that minimizes the opponent’s maximum payoff.

In the 2048-puzzle game, the computer is technically not "adversarial", all it does is spawn random tiles of 2 and 4 each turn, with a designated probability of either a 2 or a 4; it certainly does not specifically spawn tiles at the most inopportune locations to foil the player’s progress. However, an AI can be created to play as if the computer is completely adversarial. In particular in this project the Minimax algorithm will be employed by assuming that the computer is adverserial.

In game-playing we generally pick a strategy to employ. With the Minimax algorithm, the strategy assumes that the computer opponent is perfect in minimizing the player’s outcome. Whether or not the opponent is actually perfect in doing so is another question. As a general principle, how far the actual opponent’s actual behavior deviates from the assumption certainly affects how well the AI performs. However, it can be seen that this strategy works well in this game.

The player AI agent is assumed to be the max player, thus will try to maximize its score.

As in the case of a simple game like tic-tac-toe, it is useful to employ the minimax algorithm, which assumes that the opponent is a perfect “minimizing” agent. In practice, however, we may encounter a sub-par opponent that makes silly moves. When this happens, the algorithm’s assumption deviates from the actual opponent’s behavior. In this case, it still leads to the desired outcome of never losing. However, if the deviation goes the other way (e.g. suppose we employ a "maximax" algorithm that assumes that the opponent wants us to win), then the outcome would certainly be different.

# Alpha-beta pruning
The Alpha-beta pruning algorithm is an expansion of Minimax, which heavily decreases (prunes) the number of nodes that we must evaluate/expand. To achieve this, the algorithm estimates two values the alpha and the beta. If in a given node the beta is less than alpha then the rest of the subtrees can be pruned.

This needs to be implemented to speed up the search process by eliminating irrelevant branches. Since the search space is huge (the AI agent can take any of the 4 actions LEFT, RIGHT, TOP, DOWN and the computer player can choose any free tile randomly and fill it with either 2 or 4), it will be very expensive to search all the branches.

# Heuristic
In order to use the above algorithms we must first identify the two players. The first player is the person who plays the game. The second player is the computer which randomly inserts values in the cells of the board. Obviously the first player tries to maximize his/her score and achieve the 2048 merge. On the other hand the computer in the original game is not specifically programmed to block the user by selecting the worst possible move for him but rather randomly inserts values on the empty cells.

So why we use AI techniques which solve zero-sum games and which specifically assume that both players select the best possible move for them? The answer is simple; despite the fact that it is only the first player who tries to maximize his/her score, the choices of the computer can block the progress and stop the user from completing the game. By modeling the behavior of the computer as an orthological non-random player we ensure that our choice will be a solid one independently from what the computer plays.

The second important part is to assign values to the states of the game. This problem is relatively simple because the game itself gives us a score. Unfortunately trying to maximize the score on its own is not a good approach. One reason for this is that the position of the values and the number of empty valued cells are very important to win the game. For example if we scatter the large values in remote cells, it would be really difficult for us to combine them. Additionally if we have no empty cells available, we risk losing the game at any minute.

For all the above reasons, several heuristics have been suggested such as the Monoticity and the Free Tiles of the board. The main idea is not to use the score alone to evaluate each game-state but instead construct a heuristic composite score that includes the aforementioned scores.

Since we likely want to include more than one heuristic evaluation functions, it will be needed to assign weights associated with each individual heuristic. Deciding on an appropriate set of weights will take careful reasoning, along with careful experimentation. The typical heuristic functions used use the following intuitive ideas:

1. Takes into account the Monotonicity property: this tries to ensure that there is no high value stacked in between the small values preventing the merging of the tiles and resulting in a premature end of the game with low score.

2. Takes into account the number of free tiles left: if there is low number of free available tiles, then it’s likely that the game will end with low premature scores, so this heuristic tries to ensure that the player AI agent chooses the move that allows more free spaces by looking ahead.

### Contributions

[Cricco95](https://github.com/sigod) is the maintainer for this repository.

Other notable contributors:

 - [TimPetricola](https://github.com/TimPetricola) added best score storage
 - [chrisprice](https://github.com/chrisprice) added custom code for swipe handling on mobile
 - [marcingajda](https://github.com/marcingajda) made swipes work on Windows Phone
 - [mgarciaisaia](https://github.com/mgarciaisaia) added support for Android 2.3
 - [Cricco95](https://github.com/sigod) added the AI

Many thanks to [rayhaanj](https://github.com/rayhaanj), [Mechazawa](https://github.com/Mechazawa), [grant](https://github.com/grant), [remram44](https://github.com/remram44) and [ghoullier](https://github.com/ghoullier) for the many other good contributions.

### Screenshot

<p align="center">
  <img src="https://cloud.githubusercontent.com/assets/1175750/8614312/280e5dc2-26f1-11e5-9f1f-5891c3ca8b26.png" alt="Screenshot"/>
</p>

That screenshot is fake, by the way. I never reached 2048 :smile:

## Contributing
Changes and improvements are more than welcome! Feel free to fork and open a pull request. Please make your changes in a specific branch and request to pull into `master`! If you can, please make sure the game fully works before sending the PR, as that will help speed up the process.

You can find the same information in the [contributing guide.](https://github.com/gabrielecirulli/2048/blob/master/CONTRIBUTING.md)

## License
2048 is licensed under the [MIT license.](https://github.com/gabrielecirulli/2048/blob/master/LICENSE.txt)
