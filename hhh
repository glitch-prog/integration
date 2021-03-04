# integration
#include <SFML/Graphics.hpp>
#include <SFML/Audio.hpp>
#include <iostream>
#include <vector>
#include "Game.hpp"

using namespace std;
using namespace sf;
// 11.
/*
Класс кнопки
*/
class Button {
private:
	// 12.
	// Прямоугольник, описывающий кнопку
	sf::RectangleShape buttonRect;
	// Текст на кнопке
	sf::Text buttonText;
public:
	/* 13.
	Кноструктор с параметрами для создания кнопки
	*/
	Button(std::string name, sf::Vector2f &position, sf::Vector2f &size, sf::Font &buttonFont) {
		// настраиваем прямоугольник для кнопки
		this->buttonRect.setSize(size);
		this->buttonRect.setOrigin(size.x / 2, 0);
		this->buttonRect.setFillColor(sf::Color(141, 0, 0));
		this->buttonRect.setOutlineColor(sf::Color::Red);
		this->buttonRect.setOutlineThickness(3);

		// настраиваем шрифт для кнопки
		this->buttonText.setFont(buttonFont);
		this->buttonText.setStyle(sf::Text::Bold);
		this->buttonText.setString(name);
		this->buttonText.setFillColor(sf::Color::Blue);
		this->buttonText.setCharacterSize(56.25);
		this->buttonText.setLetterSpacing(2);

		//  buttonText.getLocalBounds() - метод, который возвращает размеры текста (ширину и выстоу)
		float widthOfMenuText = buttonText.getLocalBounds().width;
		this->buttonText.setOrigin(widthOfMenuText / 2, 0);

		this->setPosition(position.x, position.y);
	}

	/* 14.
	Метод установки позиции кнопки
	*/
	void setPosition(float x, float y) {
		this->buttonRect.setPosition(x, y);
		this->buttonText.setPosition(x, y);
	}

	/* 18.
	Метод, проверяющий, находится ли точка внутри кнопки
	*/
	bool isPointInside(sf::Vector2i& point) {
		// this->buttonRect.getGlobalBounds() - это метод, который позволяет узнать
		// информацию о прямоугольнике (позицию и размер прямоугольника) this->buttonRect.
		// contains - это специальный метод SFML, который позволяет узнать, 
		// нахдится ли точка внутри прямоугольника, который вернулся методом getGlobalBounds()
		return this->buttonRect.getGlobalBounds().contains(point.x, point.y);
	}

	/*
	Метод draw отрисовывает прямоугольник кнопки и текст на ней
	*/
	void draw(sf::RenderWindow & window) {
		window.draw(this->buttonRect);
		window.draw(this->buttonText);
	}
};

struct GameResources {
	sf::Music menuMusic;
};

GameResources GAME_RESOURCES;

/* 8.
Максимально упрощенная версия меню
*/
class Menu {
private:
	// 9.
	// Вектор, в котором храняться все кнопки меню
	std::vector <Button> menuButtons;
	// переменная, котороя определяет открыто или закрыто меню
	bool showMenu = true;

	sf::Texture backgroundTexture;
public:
	// Штука, которая просто нужна:)
	Menu() = default;

	/* 10.
	Конструктор с параметрами для меню,
	Первый параметр - названия для кнопок
	Второй параметр - шрифт для кнопок
	Третий - размер окна
	*/
	Menu(std::vector <std::string> &menuNames, sf::Font& baseFontForGame, sf::Vector2u &sizeOfWindow) {
		int countOfMenuButtons = menuNames.size();

		// Задаем размеры для кнопок
		float buttonWidth = 500;
		float buttonHeight = 62.5;

		// В зависимости от того, сколько мы названий задали для кнопок, столько их и будет создано
		for (int i = 0; i < countOfMenuButtons; i++) {
			// Позиции кнопок по x и y координатам - по x они будут расположены посередине
			float buttonPositionX = sizeOfWindow.x / 4;
			float buttonPositionY = 600 + i * (buttonHeight + 20);

			// Создаем кнопку с помощью конструктора с параметрами, в который мы передаем
			// menuNames[i] - название кнопки
			// sf::Vector2f(buttonPositionX, buttonPositionY) - координаты кнопки,
			// sf::Vector2f(buttonWidth, buttonHeight) - размер кнопки
			// baseFontForGame - шрифт кнопки
			Button button(
				menuNames[i],
				sf::Vector2f(buttonPositionX, buttonPositionY),
				sf::Vector2f(buttonWidth, buttonHeight),
				baseFontForGame
			);

			// Созданную кнопку button добавляем в вектор menuButtons, 
			// где храняться все кнопки
			this->menuButtons.push_back(button);
		}

		this->loadImageForBackground();
		this->loadmusic();
	}

	bool isMenuOpen() {
		return this->showMenu;
	}

	void loadImageForBackground() {
		if (this->backgroundTexture.loadFromFile("fonts/w.png") == false) {
			std::cout << "Can't find the font file" << std::endl;
		}
	}
	
	void loadmusic() {
		/*
		sf::SoundBuffer buffer;
		buffer.loadFromFile("Music/background.wav");// тут загружаем в буфер что то
		sf::Sound sound;
		sound.setBuffer(buffer);
		sound.play();
		*/

		if (!GAME_RESOURCES.menuMusic.openFromFile("Music/999.flac")) {
			std::cout << "Error";
			return;
		}

		GAME_RESOURCES.menuMusic.play();
	}

	/*
	Метод отрисовки меню
	*/
	void draw(sf::RenderWindow &window) {
		// Если окно закрыто, тоне рисуем меню - просто прекращаем работу метода
		if (this->showMenu == false) {
			return;
		}

		this->drawBackground(window);

		// Рисуем наши кнопки
		for (int i = 0; i < menuButtons.size(); i++) {
			menuButtons[i].draw(window);
		}
	}
	
	void drawBackground(sf::RenderWindow &window) {
		sf::RectangleShape rect;
		rect.setTexture(&this->backgroundTexture);
		rect.setSize(sf::Vector2f(window.getSize()));
		window.draw(rect);
	}

	void open() {
		this->showMenu = true;
	}

	void close() {
		this->showMenu = false;
		GAME_RESOURCES.menuMusic.stop();
	}

	/* 17.
	Метод определения, кликнута ли кнопка с номером, который передается
	через параметр numberOfMenu. Второй параметр - point, которая проверяется
	на нахождение внутри кнопки.
	Метод возвращает true, если точка point находится внутри кнопки с номером
	numberOfMenu, иначе - false
	*/
	bool isPointInsideButton(int numberOfMenu, sf::Vector2i& point) {
		int countOfButtons = this->menuButtons.size();

		// Если был указан номер кнопки, которого нет, то выводим сообщение об ошибке
		if (numberOfMenu < 0 || numberOfMenu >= countOfButtons) {
			std::cout << "Error: Incorrect number of menu button!" << std::endl;
			return false;
		}

		// С помощью this->menuButtons[numberOfMenu] мы извлекаем кнопку с номером numberOfMenu,
		// У этой кнопки вызываем метод, который определяет определяет, является ли точка point
		// нее.
		return this->menuButtons[numberOfMenu].isPointInside(point);
	}
};

/*
2.
Собственный тип данных, который описывает нашу игру
*/
class Game {
private:
	enum class GAME_MODE {MENU, PLAY_GAME};
	/* 3.
	Шрифт для игры
	*/
	sf::Font baseFontForGame;
	/* 4.
	Переменная, в которой храняться кнопки меню. Menu - это тип данных,
	созданный нами.
	*/
	Menu mainMenu;

	GameInitiator gameInitiator;

	// Режим игрый 1 - это меню, 2 - это игра
	GAME_MODE gameMode = GAME_MODE::MENU;
public:
	/*  5.
	Конструктор с параметрами, который принимает размер окна
	нашей игры
	*/
	Game(sf::Vector2u &sizeOfWindow) {
		// Вызываем метод по загрузке шрифта, который будет использоваться для кнопок
		this->loadFontForGame();

		// Вызываем метод, который создает кнопки для нашего меню
		this->initializeMainMenu(sizeOfWindow);
	}


	// 20.
	void draw(sf::RenderWindow &window) {
		switch (this->gameMode) {
			case GAME_MODE::MENU: {
				this->mainMenu.draw(window);
				break;
			}
			case GAME_MODE::PLAY_GAME: {
				this->gameInitiator.draw(window);
				break;
			}
		}
	}

	inline void processGameActions() {
		if (this->gameMode == GAME_MODE::PLAY_GAME) {
			this->gameInitiator.processActions();
		}
	}

	/* 16.
	Метод, который обрабатывает события, которые происходят в игре.
	Для этого нужно наше окно window, которое передается первым параметром,
	и переменная события, с помощью которой можно определить, какое событие произошло
	(допустим, кто-то кликнул мышкой по кнопке - это событие клика мыши)
	*/
	inline void processGameEvents(sf::RenderWindow &window, sf::Event &event) {
		if (event.type == sf::Event::Closed) {
			window.close();
		}

		switch (this->gameMode) {
			case GAME_MODE::MENU: {
				this->processMenuEvents(window, event);
				break;
			}
			case GAME_MODE::PLAY_GAME: {
				this->gameInitiator.processEvents(event);
				break;
			}
		}
	}
private:
	/* 7.
	Метод, позволяющий создать меню для игры
	*/
	void initializeMainMenu(sf::Vector2u &sizeOfWindow) {
		std::vector <std::string> menuNames{
			"poshel nahui",
			"Training",
			"Exit",
		};

		/*
		С помощью конструктора с параметрами создаем меню,
		у которой кнопки будут с именами, которые сохранены в вектор
		menuNames, для кнопок будет использоваться шрифт игры. Третий
		параметр - это размер окна нашей игры - он нужен для того, чтобы
		разместить меню по центру окна.
		*/
		Menu mainMenu(menuNames, this->baseFontForGame, sizeOfWindow);

		// Записываем созданное меню в поле mainMenu
		this->mainMenu = mainMenu;
	}

	/* 6.
	Этот метод предназначен для загрузки шрифта из папки fonts.
	Метод приватный, то есть не доступен через точку для переменной
	mySuperMegaGame, расположенной в main(). Это сделано так потому, так
	как нет нужды, чтобы он был доступен через точку для mySuperMegaGame.
	Если когда-нибудь эта нужда появится, то переместить функцию в public - дело
	2 секунд.
	*/
	void loadFontForGame() {
		// Берем наше поля шрифта baseFontForGame и загружаем его.
		// В случае ошибки выдаем сообщение.
		// Без шрифта никакой текст не выведется вообще.
		if (this->baseFontForGame.loadFromFile("fonts/10967.otf") == false) {
			std::cout << "Can't find the font file" << std::endl;
		}

	}

	void processMenuEvents(sf::RenderWindow &window, sf::Event &event) {
		/*
		В методе initializeMainMenu мы создали определенные кнопки меню,
		которые у нас будут в меню. Следующие переменные хранят номера этих
		элементов меню, начиная с 0.
		*/
		int START = 0;
		int HELP = 1;
		int EXIT = 2;

		/*
		Если была нажата левая кнопка мыши
		*/
		if (sf::Mouse::isButtonPressed(sf::Mouse::Button::Left)) {
			// Получаем позицию курсора
			sf::Vector2i point = sf::Mouse::getPosition(window);

			// Если курсор мыши при клике находиться внутри кнопки START
			// (кнопка под номером 0)
			if (this->mainMenu.isPointInsideButton(START, point)) {
				std::cout << "Button 111 is clicked!" << std::endl;
				this->mainMenu.close();
				this->gameMode = GAME_MODE::PLAY_GAME;
			}

			// Если курсор мыши при клике находиться внутри кнопки HELP
			// (кнопка под номером 1)
			if (this->mainMenu.isPointInsideButton(HELP, point)) {
				std::cout << "Button 2 is clicked!" << std::endl;
			}

			// Если курсор мыши при клике находиться внутри кнопки EXIT
			// (кнопка под номером 2)
			if (this->mainMenu.isPointInsideButton(EXIT, point)) {
				std::cout << "Button 3 is clicked!" << std::endl;
			}
		}
	}
};

int main()
{
	sf::RenderWindow window(
		sf::VideoMode(1920, 1080),
		"Example of menu",
		sf::Style::Default,
		sf::ContextSettings(0, 0, 8)
	);

	// 1.
	// Создаем переменную нашей игры, для корректной работы.
	// С помощью конструктора с параметрами, передаем ему
	// размеры нашего окна (для этого есть метод window.getSize()).
	// Она возвращает переменную типа данных sf::Vector2u, у которой
	// есть 2 поля - x и y. x хранит ширину, а y - высоту окна
	
	Game mySuperMegaGame(window.getSize());

	while (window.isOpen())
	{
		sf::Event event;

		while (window.pollEvent(event))
		{
			// 15.
			// у нашей игры есть метод processGameEvents, который
			// обрабатывает события, которые происходят в системе (клики мыши, и т.д.)
			mySuperMegaGame.processGameEvents(window, event);
		}



		window.clear();

		mySuperMegaGame.processGameActions();

		
		mySuperMegaGame.draw(window);

		window.display();
	}

	return 0;
}
