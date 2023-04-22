# 2) What problems / warnings are there with code?
Ans: There are multiple problems/warnings with the code that are to be rectified to render the component correctly.

#### 1. Uncaught TypeError: PropTypes.shapeOf is not a function, the propTypes declaration for the items prop in the WrappedListComponent component is wrong.<br>
<br>Code Given :
> WrappedListComponent.propTypes = {<br>
  items: PropTypes.array(PropTypes.shapeOf({ //ShapeOf is not a Valid Function<br>
    text: PropTypes.string.isRequired,<br>
  })),<br>
 };

 Edited Code :<br>
> WrappedListComponent.propTypes = {<br>
  items: PropTypes.arrayOf(PropTypes.shape({ //Correct Function is ArrayOf<br>
    text: PropTypes.string.isRequired,<br>
  })),<br>
 };<br>


#### 2. The onClickHandler property in WrappedSingleList Item is called incorrectly; it should be called as a callback to be triggered when the list item is clicked.

Code Given :
> const WrappedSingleListItem = ({<br>
  index,<br>
  isSelected,<br>
  onClickHandler,<br>
  text,<br>
}) => {<br>
  return (<br>
    <li<br>
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}<br>
      onClick={onClickHandler(index)}> //callback is not defined 
      {text}<br>
    </li><br>
  );<br>
};<br>

Edited Code :
> const WrappedSingleListItem = ({<br>
  index,<br>
  isSelected,<br>
  onClickHandler,<br>
  text,<br>
}) => {<br>
  return (<br>
    <li<br>
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}<br>
      onClick={()=>onClickHandler(index)} //with call back function ><br>
      {text}<br>
    </li><br>
  );<br>
<br>

#### 3. The default value of null for the items prop in WrappedListComponent can lead to issues when attempting to perform mapping operations on it.

provided code:
> WrappedListComponent.defaultProps = {<br>
  items: null,<br>
};<br>

Modified Code:
> WrappedListComponent.defaultProps = {<br>
  items:  [<br>
    {text : "L Chetan"},<br>
    {text : "12012893"},<br>
    {text : "LPU"},<br>
    {text : "B-Tech"},<br>
    {text : "Cse"},   <br>
  ],
};<br>

#### 4. To ensure that only the clicked option changes color and not all the content present in a list, we can modify the isSelected property to isSelected = (SelectedIndex === Index). This will allow us to update the selected option's color based on its index in the list.

Code Given :
> return (<br>
    <ul style={{ textAlign: 'left' }}><br>
      {items.map((item, index) => (<br>
        <SingleListItem<br>
          onClickHandler={() => handleClick(index)}<br>
          text={item.text}<br>
          index={index}<br>
          isSelected={selectedIndex}// using this syntax, all available colours are selected and changed.<br>
        /><br>
      ))}<br>
    </ul>
  )<br>
};<br>

Edited Code :
> return (<br>
    <ul style={{ textAlign: 'left' }}><br>
      {items.map((item, index) => (<br>
        <SingleListItem<br>
          onClickHandler={() => handleClick(index)}<br>
          text={item.text}<br>
          index={index}<br>
          isSelected={selectedIndex=== index}// Whenever a user selects an option, only that option's colour changes.        /><br>
      ))}<br>
    </ul>
  )<br>
};<br>

The modification to the isSelected property ensures that only the option selected by the user changes its color, while the rest of the content in the list remains unaffected.<br>
            
#### 5). Uncaught TypeError: setSelectedIndex is not a function in the code given. The useState function used in the WrappedListComponent component initialises the selectedIndex state, but the initial value (null) should appear first in the returned array, not the setter function.

Given:
> const [setSelectedIndex, selectedIndex] = useState(); //incorrect sequence

Edited Code:
> const [ selectedIndex, setSelectedIndex] = useState(); //corrected code


#### 6. In the map function, the value of the unique key is not defined as a prop for each child value.

Code Given :
> return (<br>
    <ul style={{ textAlign: 'left' }}><br>
      {items.map((item, index) => (<br>
        <SingleListItem //key is not present<br>
          onClickHandler={() => handleClick(index)}<br>
          text={item.text}<br>
          index={index}<br>
          isSelected={selectedIndex=== index}<br>
        /><br>
      ))}<br>
    </ul>
  )<br>
};
<br>
Modified Code:

> return (<br>
    <ul style={{ textAlign: 'left' }}><br>
      {items.map((item, index) => (<br>
        <SingleListItem<br>
        key={index}// added key<br> 
          onClickHandler={() => handleClick(index)}<br>
          text={item.text}<br>
          index={index}<br>
          isSelected={selectedIndex=== index}<br>
        /><br>
      ))}<br>
    </ul>
  )<br>
};<br>

# 3) Please fix, optimize, and/or modify the component as much as you think is necessary.
            
Ans: The Working Modified Code:<br><br>
import React, { useState, useEffect, memo } from 'react';<br>
import PropTypes from 'prop-types';<br>

// Single List Item<br>
const WrappedSingleListItem = ({<br>
  index,<br>
  isSelected,<br>
  onClickHandler,<br>
  text,<br>
}) => {<br>
  return (<br>
    <li<br>
      style={{ backgroundColor: isSelected ? 'green' : 'red' }}<br>
      onClick={() => onClickHandler(index)} >//Modified<br>
      {text}<br>
    </li><br>
  );<br>
};<br>

WrappedSingleListItem.propTypes = {<br>
  index: PropTypes.number,<br>
  isSelected: PropTypes.bool,<br>
  onClickHandler: PropTypes.func.isRequired,<br>
  text: PropTypes.string.isRequired,<br>
};<br>

const SingleListItem = memo(WrappedSingleListItem);<br>

// List Component<br>
const WrappedListComponent = ({<br>
  items,<br>
}) => {<br>
  const [selectedIndex, setSelectedIndex] = useState(); //Modified<br>

  useEffect(() => {<br>
    setSelectedIndex(null);<br>
  }, [items]);<br>

  const handleClick = index => {<br>
    setSelectedIndex(index);<br>
  };<br>

  return (<br>
    <ul style={{ textAlign: 'left' }}><br>
      {items.map((item, index) => (<br>
        <SingleListItem<br>
          key={index} //Modified<br>
          onClickHandler={() => handleClick(index)}<br>
          text={item.text}<br>
          index={index}<br>
          isSelected={selectedIndex === index} //Modified<br>
        /><br>
      ))}<br>
    </ul><br>
  )<br>
};<br>

WrappedListComponent.propTypes = {<br>
  items: PropTypes.arrayOf(PropTypes.shape({ //Modified<br>
    text: PropTypes.string.isRequired,<br>
  })),<br>
};<br>

WrappedListComponent.defaultProps = { //Modified<br>
  items: [<br>
    { text: "L Chetan" },<br>
    { text: "12012893" },<br>
    { text: "LPU" },<br>
    { text: "B-Tech" },<br>
    { text: "Cse" },<br>

  ],<br>
};<br>
const List = memo(WrappedListComponent);<br>
export default List;<br>
