# Frontend Engineer Assignment
##  STEELEYE


- **Name : Pankaj kumar**
- **Reg-no: 11915560**

#### 1. Explain what the simple List component does.

Ans:   when we click on any list element onclick event raise the event and set the index ( to `selectedIndex`)  and this selectedIndex matched with an index that we clicked on if it’s true then the color of the list item will get the change from red to green also. when there is no list item we cannot map to `WrappedSingleListItem`   we are iterating on item list and for every item we are rendering on `SingleListItem` component 

#### 2. What problems/warnings are there with the code?

- **Problem - 1**: Cannot read properties of null (reading 'map')  occurs when we call the map() method on a null value, most often when initializing a state variable to null  How to resolve: initialize the value you're mapping over to an empty array
- **Problem - 2**: shapeOf is not a function instead of this use shape and instead of `PropTypes.array` is a very generic type. It could be an array of literally anything. But in this code, we are passing arguments so that’s why we should use `PropTypes.arrayOf` 
- **Problem - 3**: `isSelected` of type `number` supplied to `WrappedSingleListItem`, expected `boolean`. To solve this we compare `selectedindex` with current `index` `isSelected={selectedIndex === index}` so this give us boolean value and according to that change the `color` green or red
- **Problem - 4**:   Each child in a list should have a unique `key` prop so we need to give assign the `key` for every list component 
- **Problem - 5**:  Cannot update a component (`WrappedListComponent`) while rendering a different component (`WrappedSingleListItem`) because we cannot pass the function inside the `onclickhandler` prop instead pass the `setselectedindex` and this will pass to onclick event and assign the index to `selectedIndex`.

#### 3  .Please fix, optimize, and/or modify the component as much as you think is necessary.

``` javascript 
import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item
const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? "green" : "red" }}
      onClick={() => onClickHandler(index)}
    >
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
const WrappedListComponent = ({ items }) => {
  const [selectedIndex, setSelectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  // const handleClick = (index) => {
  //   setSelectedIndex(index);
  // };

  return (
    <ul style={{ textAlign: "left" }}>
      {items?.map((item, index) => (
        <SingleListItem
          key={index}  //assiging the key to every list element 
          onClickHandler={setSelectedIndex}
          text={item.text}
          index={index}
          isSelected={selectedIndex === index} // checking the selected index with index 
        />
      ))}
    </ul>
  );
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ),
};

WrappedListComponent.defaultProps = {
  items: null
};

const List = memo(WrappedListComponent);

export default List;
```
