var months = [
  "January",
  "February",
  "March",
  "April",
  "May",
  "June",
  "July",
  "August",
  "September",
  "October",
  "November",
  "December",
];

// grabbing various elements
// not buttons
const body = document.body;
const footer = document.querySelector("footer");
const createMenu = document.querySelector(".create-menu");
const imageInput = document.getElementById("upload-image-input");
const selectedImage = document.getElementById("selected-image");
const createDate = document.querySelector(".create-date");
const createDescription = document.querySelector("textarea");
const postMemoButton = document.getElementById("post-memo-button");
const warningContainer = document.getElementById("warning-container");

// buttons
const newMemoButton = document.querySelector(".button-container");
const closeMenuButton = document.querySelector(".close-menu-button");
const closeImageButton = document.querySelector(".close-image-button");
const warningButton = document.querySelector(".warning-button");
const cancelButton = document.querySelector(".cancel-button");

// adding event listeners
newMemoButton.addEventListener("click", openMenu);
closeMenuButton.addEventListener("click", showWarning);
imageInput.addEventListener("change", (e) => loadImageFile(e));
closeImageButton.addEventListener("click", clearImage);
createDescription.addEventListener("keyup", updateData);
postMemoButton.addEventListener("click", postNewMemo);
warningButton.addEventListener("click", closeMenu);
cancelButton.addEventListener("click", closeWarning);

// navbar scrolling always active
navbarScroll();

// clicking outside warning overlay
warningContainer.addEventListener("click", (e) => {
  if (e.target === e.currentTarget) closeWarning();
});

// generate date for new memos
window.onload = () => {
  createDate.innerHTML += "Publish Date:  " + generateCurrentDate();
};

/**
 * Updates data within the "Create New Memo" menu.
 * Called every time data is modified inside menu.
 */
function updateData() {
  // check if there is valid data within the "Create New Memo" menu
  if (!createPostHasData()) {
    // disables the "Post Memo" button
    postMemoButton.classList.add("disabled");

    //remove overlay, enable form, disable post memo button
    selectedImage.style.visibility = "hidden";
    closeImageButton.style.visibility = "hidden";
    document.getElementById("upload-image-input").disabled = false;
  } else {
    // there is either text, image or both

    // enable the "Post Memo" button
    postMemoButton.classList.remove("disabled");

    if (selectedImage.getAttribute("src") == "") {
      // there is no image ==[means]==> there is text.
      //remove overlay, enable form
      selectedImage.style.visibility = "hidden";
      closeImageButton.style.visibility = "hidden";
      document.getElementById("upload-image-input").disabled = false;
    } else {
      // there is or isn't text ==[means]==> there is an image
      // enable overlay, disable form
      selectedImage.style.visibility = "visible";
      closeImageButton.style.visibility = "visible";
      document.getElementById("upload-image-input").disabled = true;
    }
  }
}

/**
 * Called every time data is modified inside "Create New Memo" menu.
 */
function togglePostMemoButton() {
  if (createPostHasData()) {
    postMemoButton.classList.remove("disabled");
  } else {
    postMemoButton.classList.add("disabled");
  }
}

/**
 * Checks if there is valid data within the "Create New Memo" menu.
 * Returns true if there is either text, an image or both. False otherwise.
 */
function createPostHasData() {
  return (
    selectedImage.getAttribute("src") != "" ||
    createDescription.value ||
    createDescription.value.length > 0
  );
}

/**
 *  Called when user clicks "Select Image" input.
 */
function loadImageFile(event) {
  // display image
  selectedImage.src = URL.createObjectURL(event.target.files[0]);
  selectedImage.onload = () => {
    URL.revokeObjectURL(selectedImage.src);
  };
  updateData();
}

/**
 * Called when the user clears the image inside "Create New Memo" menu.
 */
function clearImage() {
  // remove image
  selectedImage.src = "";
  updateData();
}

/**
 * Generate the current date in the format [Month] [DD], [YYYY].
 */
function generateCurrentDate() {
  // formatting the date
  const date = new Date();
  var numberMonth = date.getMonth();
  var currentMonth = months[numberMonth]; // convert to word
  var currentDate = date.getDate();
  var currentYear = date.getFullYear();

  return (today = currentMonth + " " + currentDate + ", " + currentYear);
}

/**
 *  Occurs when clicking on the "Post Memo" button.
 */
function postNewMemo() {
  // make post container
  const postContainer = document.querySelector(".post-container");
  const post = document.createElement("div");
  post.setAttribute("class", "post");

  // make the image
  var image = selectedImage.getAttribute("src");
  if (image) {
    const postImage = document.createElement("img");
    postImage.setAttribute("src", image);
    postImage.setAttribute("class", "post-image");
    post.appendChild(postImage); // append
  }

  // make the date
  var today = generateCurrentDate();
  const postDate = document.createElement("div");
  postDate.setAttribute("class", "post-date");
  postDate.innerHTML += today;
  post.appendChild(postDate); // append

  // make the description
  var description = createDescription.value;
  if (description) {
    const postDesc = document.createElement("p");
    postDesc.setAttribute("class", "post-description");
    postDesc.innerHTML += description;
    post.appendChild(postDesc); // append
  }

  postContainer.prepend(post); // append to the feed

  // clean up
  closeMenu(); // close new memo menu
  window.scrollTo(0, 0); // scroll to top, where new post is
  setTimeout(flashNewPost, 1000); // notify user of new post
}

/**
 * Clears all data from "Create New Memo" menu.
 */
function clearCreatePostData() {
  // timer to sync with css transition
  setTimeout(() => {
    createDescription.value = "";
    clearImage();
  }, 500);
}

/**
 * Opens "Create New Memo" menu.
 */
function openMenu() {
  createMenu.classList.add("active");
  body.classList.add("noscroll"); //disable feed from scrolling

  changeDate(); // update date
  clearCreatePostData(); // clear menu
}

/**
 * Closes "Create New Memo" menu.
 */
function closeMenu() {
  createMenu.classList.remove("active");
  body.classList.remove("noscroll"); //enable feed scrolling

  closeWarning(); // warning closes too
  clearCreatePostData(); // clear menu
}

/**
 * Changes the date in "Create New Memo" menu.
 */
function changeDate() {
  createDate.innerHTML = "";
  createDate.innerHTML += "Publish Date:  " + generateCurrentDate();
}

/**
 * Opens warning popup within "Create New Memo" menu.
 */
function showWarning() {
  if (createPostHasData()) warningContainer.classList.add("active");
  else closeMenu();
}

/**
 * Closes warning popup within "Create New Memo" menu.
 */
function closeWarning() {
  warningContainer.classList.remove("active");
}

/**
 * Flashes post twice upon creating a new memo.
 */
function flashNewPost() {
  var newPost = document.querySelector(".post-container").firstChild;
  newPost.style.animation = "0.5s 2 pulseBg";
}

/**
 * Navbar interaction - dissapates when scrolling down, reappears when scrolling up.
 */
function navbarScroll() {
  let nav = document.getElementById("nav");
  let lastScrollTop;

  window.addEventListener("scroll", () => {
    var distFromTop = window.pageYOffset || document.documentElement.scrollTop;
    if (distFromTop > lastScrollTop) {
      nav.style.opacity = 0;
    } else {
      nav.style.opacity = 1;
    }
    lastScrollTop = distFromTop;
    nav.style.top = `${distFromTop}px`;
  });
}
