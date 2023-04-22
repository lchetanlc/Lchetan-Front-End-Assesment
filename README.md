# 2) What problems / warnings are there with code?
Ans: There are multiple problems/warnings with the code that are to be rectified to render the component correctly.

#### 1. Uncaught TypeError: PropTypes.shapeOf is not a function, the propTypes declaration for the items prop in the WrappedListComponent component is wrong.<br>
<br>Code Given :
<pre>
WrappedListComponent.propTypes = {
  items: PropTypes.array(PropTypes.shapeOf({ //ShapeOf is not a Valid Function
    text: PropTypes.string.isRequired,
  })),
 };
</pre>
 Edited Code :
<pre>
WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({ //Correct Function is ArrayOf
    text: PropTypes.string.isRequired,
  })),
 };
</pre>

#### 2. The onClickHandler property in WrappedSingleList Item is called incorrectly; it should be called as a callback to be triggered when the list item is clicked.

Code Given :
<pre>
const WrappedSingleListItem = ({
  index,
  isSelected,
  onClickHandler,
  text,
}) => {
  return (
    <li<br>
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={onClickHandler(index)}> //callback is not defined 
      {text}
    </li>
  );
};
</pre>
Edited Code :
<pre>
const WrappedSingleListItem = ({
  index,
  isSelected,
  onClickHandler,
  text,
}) => {
  return (
    <li<br>
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={() => onClickHandler(index)}> //callback is not defined 
      {text}
    </li>
  );
};
</pre>

#### 3. The default value of null for the items prop in WrappedListComponent can lead to issues when attempting to perform mapping operations on it.

provided code:
<pre>
WrappedListComponent.defaultProps = {
  items: null,
};
</pre>

Modified Code:
<pre>
WrappedListComponent.defaultProps = {
  items:  [
    {text : "L Chetan"},
    {text : "12012893"},
    {text : "LPU"},
    {text : "B-Tech"},
    {text : "Cse"},   
  ],
};
</pre>

#### 4. To ensure that only the clicked option changes color and not all the content present in a list, we can modify the isSelected property to isSelected = (SelectedIndex === Index). This will allow us to update the selected option's color based on its index in the list.

Code Given :
<pre>
return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}// using this syntax, all available colours are selected and changed.
        />
      ))} </ul>
  )};
</pre>
Edited Code :
<pre>
 return (
    <ul style={{ textAlign: 'left' }};
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex=== index}// Whenever a user selects an option, only that option's colour changes.        
          />
      ))} </ul>
  )};
</pre>

The modification to the isSelected property ensures that only the option selected by the user changes its color, while the rest of the content in the list remains unaffected.
            
#### 5). Uncaught TypeError: setSelectedIndex is not a function in the code given. The useState function used in the WrappedListComponent component initialises the selectedIndex state, but the initial value (null) should appear first in the returned array, not the setter function.

Given:
<pre>
 const [setSelectedIndex, selectedIndex] = useState(); //incorrect sequence
</pre>
Edited Code:
<pre>
const [ selectedIndex, setSelectedIndex] = useState(); //corrected code
</pre>


#### 6. In the map function, the value of the unique key is not defined as a prop for each child value.

Code Given :
<pre>
return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem //key is not present
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex=== index} />
      ))}</ul>
  )};

</pre>
Modified Code:
<pre>
 return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
        key={index}// added key
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex=== index} />
      ))} </ul>
  )};
</pre>
# 3) Please fix, optimize, and/or modify the component as much as you think is necessary.
            
Ans: The Working Modified Code:<br><br>
<pre>
import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
  index,
  isSelected,
  onClickHandler,
  text,
}) => {
  return (
    <li<br>
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={() => onClickHandler(index)}> //callback is not defined 
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({
  items,
}) => {
  const [selectedIndex, setSelectedIndex] = useState(); //Modified

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = index => {
    setSelectedIndex(index);
  };

  return (<br>
    &lt ul<br> style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        &ltSingleListItem
          key={index} //Modified
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex === index} //Modified
        />
      ))}<br>
    &lt/ul>
  )
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({ //Modified
    text: PropTypes.string.isRequired,
  })),
};

WrappedListComponent.defaultProps = { //Modified
  items: [
    { text: "L Chetan" },
    { text: "12012893" },
    { text: "LPU" },
    { text: "B-Tech" },
    { text: "Cse" },

  ],
};
const List = memo(WrappedListComponent);
export default List;

</pre>
