# **SEI-Project-03: 🍔 BurgerRator** 🦖

A MERN stack App to find and rate the best burgers.

**Timeframe**: One week project with [Ania Kubów](http://https://github.com/kubowania), [Chris Beaney](http://https://github.com/ChrisBeaney) and [Zeeshan Goburdhun](https://github.com/goburdhunz).

[Here you can check the website!](https://burgerrator.herokuapp.com)

![Screenshot of the App](https://imgur.com/Yhdv2hk.jpg)


## Brief:
- Full-Stack application.
- Uses an Express API to serve our data from a Mongo database.
- Consumes our API with a separate front-end  built with React.
- Has multiple relationships and CRUD functionality.
- Has automated tests.

## Technologies used:
- HTML5
- CSS 3
- JavaScript (ES6)
- Express
- React (React-DOM, React-Router-DOM, React-Toastify)
- Node.js
- Webpack (with CLI)
- Git and GitHub
- Babel
- Lodashs
- Axios
- FIleStack
- Bulma
- MongoDB
- Mongoose
- Heroku

## Approach Taken:
- **Deciding the topic:** Through brainstorming and whiteboard we decided to make an APP based on the best hamburgers  of London.
- **Division of the work:** In order to achieve the ambitious app we had on mind it was very important to divide tasks and the different parts of it. Personally, I was more involved inthe front-end of the App, working in the Home, Index, Show and About pages. I focused in the cards and filters, making them visually attractive, and easy to use. Also I designed the logo of the App. At the same time all of us created hamburgers in our "free time" so we could have a big seed dsta base.
- **Some Snippets:** 
  - Home Page :We started with the Home Page. For this it was neccesary only to show the highest 3 rated hamburgers. In the back-end it looked like this:
    ```javascript
    function homeRoute (req, res, next) {
      Burger.find()
        .sort('-rating')
        .limit(3)
        .then(burgers => res.json(burgers))
        .catch(next)
    }
    ```   
  - Login: For the login we agreed we didn't want to create a full page for the form. For this reason we learned how to create modals.
    ```javascript
       <section className="section login-portal">
        {!Auth.isAuthenticated() && <button className="button is-primary is-danger loginbutton" onClick={this.openModal}>Login</button>}
        <Modal
          isOpen={this.state.modalIsOpen}
          onAfterOpen={this.afterOpenModal}
          onRequestClose={this.closeModal}
          style={customStyles}
          contentLabel="Example Modal"
          shouldCloseOnEsc={true}
          shouldCloseOnOverlayClick={true}
        >
  ```    
  - Index Page: Applying filters to all burgers for the user to browse was a challenge. Mostly because of the ingredient filter that had to match identical strings in two arrays and the boolean search for the vegetarian (which included vegans) and the vegans, since the default of the search was to include all hamburgers containing the vegetarians and vegans.
  ```javascript
    const filterBurgers = _.filter(this.state.burgers, burger => {
      return (ingredients.length ? _.intersection(burger.ingredients, ingredients).length >= ingredients.length : true) &&
        (re.test(burger.name) || re.test(burger.restaurant.name)) && (reIng.test(burger.ingredients))  &&
        ((isVegan && burger.isVegan) ||
        (isVegetarian && burger.isVegetarian) ||
        (!isVegan && !isVegetarian))
    })
    ```     
  - Show Page: In the show page we wanted to let the authenticated users to interact with the page. Two examples of this where the rating and the comments given to each hamburger.
  ```javascript
    burgerSchema.virtual('avgUserRating')
      .get(function getAvgUserRating() {
        if(!this.comments) return 0
        return roundToHalf(this.comments.reduce((total, comment) => total + comment.userRating, 0) / this.comments.length)
      })

    burgerSchema.virtual('totalUsers')
      .get(function getTotalUsers() {
        if(!this.comments) return 0
        return this.comments.length
      })
    ```     

  ```javascript
      <div className="media-content">
        <div className="content">
          <p>
            <strong>{user.username}</strong>
            {' '}
            <small>{(new Date(createdAt)).toLocaleDateString()}</small>
            <hr className="linebreaker"/>
            {content}
          </p>
          <span className="title is-2 has-text-centered">
            <Rating
              emptySymbol= {<img src="https://i.imgur.com/931P2ih.png" className="image is-24x24"/>}
              fullSymbol= {<img src="https://i.imgur.com/f00MSST.png" className="image is-24x24"/>}
              fractions={2}
              initialRating={userRating}
              readonly
            />
          </span>
        </div>
      </div>
      {Auth.isCurrentUser(user) && <div className="media-right">
        <button id={_id} onClick={handledelete} className="delete"></button>
      </div>}
    ```    
  - External Beer API: We wanted to interact with an external API that could match with our app purpose. For this reason we decided to work with punk Api by making a request of a beer in every hamburger showpage, by just clicking the "beer match" button a modal with appear with the principal information of the beer.
  ```javascript
      componentDidMount() {
        axios.get('https://api.punkapi.com/v2/beers/random')
          .then(res => this.setState({ beer: res.data[0] }))
      }
  ```  
  
## Wins and blockers:
The teamwork communication allowed us to take the project not only to coding but to try many hamburgers by ourselves and build strong relationships. Working in team allowed us to solve our problems by communicating other which made our work more efficient and made faster de problem solving. We reinforced our collaborative coding skills by using github and git branches
and managing work and tickets using Trello. The principal challenge for me was to solve the filters the best way possible.

## Future features:
Right now the beer API shows a random beer everytime the button is clicked, I suggest that the beer shown is also based on the ingredients of the hamburger to be an even better match. The styling of the page is appropiate for the amount of time we had to build the complete app, but I think it could have its own identity and not so many default bulma styling. Other future features include:
  - To match some external API for delivery and booking will be nice for the users that want to try the highest rating hamburgers.
  - Searching for nearby burgers by grabbing window location from user.
  - Scrolling component for comments to manage long arrays of comments.
  - Login in using social accounts.

