# ImageSearch

Get Your App : **[ImageSearch App](https://tarunsingh56.github.io/ImageSearch/)** 



src/api/unsplash.js<br>
     (1)unsplash.js<br>

src/components/<br>
       (2)App.js<br>
       (3)ImageCard.js<br>
       (4)ImageList.css<br>
       (5)SearchBar.js<br>

src/index.js<br>

**Below in the code for the above stated file:**

## (1)unsplash.js

```
import axios from "axios"

export default axios.create({
    baseURL:'https://api.unsplash.com',
    headers:{
        Authorization:
        'Client-ID PKDNvmxEBb_G8baoMJ68bOyoyZHE_O4qDiKWpifUelQ '
    }
});
```

## (2)App.js

```
import React from "react";
import unsplash from "../api/unsplash";
import SearchBar from "./SearchBar";
import ImageList from "./ImageList";

class App extends React.Component{
    state={images:[]};
 onSearchSubmit=async (term)=>{
        // console.log(term);
     
      const response= await unsplash.get('/search/photos',{
            params:{query: term},
            
        });
       this.setState({images:response.data.results});
    }
    render(){
        return (
            <div className="ui center aligned container" style={{marginTop:'10px'}}>
                <SearchBar onSubmit={this.onSearchSubmit} />
                <ImageList images={this.state.images} />
            </div>
        )
    }

}

export default App;
```

## (3)ImageCard.js

```
import React from "react"

class ImageCard extends React.Component{
    constructor(props){
        super(props);
         
        this.state={spans:0};
        this.imageRef=React.createRef();
    }
    componentDidMount(){
        this.imageRef.current.addEventListener('load',this.setSpans);
    }

    setSpans=()=>{
        const height=this.imageRef.current.clientHeight;
    
         const spans=Math.ceil(height/10);
         this.setState({spans:spans});
    };
    
    render(){
        const {urls,description}=this.props.image;
        return(
            <div style={{gridRowEnd:`span ${this.state.spans}`}} >
                <img ref={this.imageRef} src={urls.regular}
                     alt={description} />
            </div>
        );
    }
}

export default ImageCard;
```

## (4)ImageList.css

```
.image-list{
    display: grid;
    grid-template-columns: repeat(auto-fill,minmax(250px,1fr));
    grid-gap: 0 10px;
    grid-auto-rows: 10px;
}


.image-list img{
    width: 250px;
    grid-row-end: span 2;
}
```

## (5)ImageList.js

```
import React from "react"
import './ImageList.css';
import ImageCard from "./ImageCard";

const ImageList=(props)=>{
    const images=props.images.map((image)=>{
        return <ImageCard key={image.id} image={image} />
    })
    return <div className="image-list">{images}</div>
}

export default ImageList;
```

## (6)SearchBar.js

```
import React from 'react';

class SearchBar extends React.Component{
    state={term:''};
    onFormSubmit=(event)=>{
        event.preventDefault();
        this.props.onSubmit(this.state.term);
    }
    render(){
        return (  
                  


                    <form onSubmit={this.onFormSubmit} className={"ui fluid category search massive "} >
                            <div className={"ui icon input "}>
                                <input className={"prompt "} type="text" value={this.state.term} onChange={e=>this.setState({term:e.target.value})} placeholder={"Image Search..."}/>
                                <i className={"search icon"}></i>
                            </div>
                             <div className={"results"}></div>
                    </form>

        );
    }
};

export default SearchBar;
```


