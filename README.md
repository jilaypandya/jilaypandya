- 👋 Hi, I’m @jilaypandya
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
jilaypandya/jilaypandya is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
#include <iostream>
#include <utility>
#include <vector>

using namespace std;

/* Implement Langton's ant:
 *
 *  https://en.wikipedia.org/wiki/Langton%27s_ant
 *
 * Hints: no UI, write testable code.
 */

/**
* Two Dimensional matrix
* Starting Point for the ant
* Number of Steps
* [b,w],[w,b]
*/
class AntMan{

  public:

  enum class Direction{
    NORTH = 0,
    WEST = 1,
    SOUTH = 2,
    EAST = 3,
  };
  /** [x,y]*/
  AntMan(std::pair<int,int> startPos, Direction startDir):currPos(startPos), currDir(startDir){ 
  }
    enum class CellType{
      WHITE = 0,
      BLACK = 1
    };

  std::pair<int,int> getCurrentPosition(){
    return this->currPos;
  }

  void moveAntBySteps(int steps, vector<vector<CellType>> &inputMap){
    /**
    * Check 1: identify the color of the cell, toggle it
    * Check 2: identify direction and move accordingly
    * Keep a track of the number of steps
    */
    for(int i=0;i<steps;i++){
      if(inputMap[currPos.first][currPos.second] == CellType::WHITE){
        inputMap[currPos.first][currPos.second] = CellType::BLACK;
        turnClockwise();
      } else {
        inputMap[currPos.first][currPos.second] = CellType::WHITE;
        turnCounterClockwise();
      }

      moveForward();
    }
  }
  private:
    std::pair<int,int> currPos;
    Direction currDir;

    void turnClockwise(){
      int myDir = (int)currDir;
      myDir++;
      myDir = myDir > (int)Direction::EAST ? (int)Direction::NORTH : myDir;
      currDir = (Direction)myDir;
    }

    void turnCounterClockwise(){
      int myDir = (int)currDir;
      myDir--;
      myDir = myDir < (int)Direction::NORTH ? (int)Direction::EAST : myDir;
      currDir = (Direction)myDir;
    }

    void moveForward(){
      switch (currDir) {
      case Direction::NORTH:
      currPos.first--;
      break;
      case Direction::SOUTH:
      currPos.first++;
      break;
      case Direction::EAST:
      currPos.second--;
      break;
      case Direction::WEST:
      currPos.second++;
      break;
      }
    }
};


int main() {

  vector<vector<AntMan::CellType>> inputGrid;

    for(int i=0;i<10;i++){
      vector<AntMan::CellType> initialLine;
      for(int i=0;i<10;i++){
        initialLine.emplace_back(AntMan::CellType::WHITE);
      }
      inputGrid.emplace_back(initialLine);
    }

  AntMan antMan(std::make_pair(5, 5),AntMan::Direction::NORTH);

  for(int i=0;i<5;i++){
    antMan.moveAntBySteps(1, inputGrid);

    std::pair<int,int> currPos = antMan.getCurrentPosition();
    std:: cout << "Step: " << i << " @ " << currPos.first << ":" << currPos.second << endl;
  }

  return 0;
}

