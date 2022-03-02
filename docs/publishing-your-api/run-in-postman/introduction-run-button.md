---
title: "Using Run in Postman buttons"
order: 108
updated: 2022-03-01
page_id: "introduction_run_button"
warning: false
contextual_links:
    - type: section
      name: "Prerequisites"
    - type: link
      name: "Grouping requests in collections"
      url: "/docs/sending-requests/intro-to-collections/"
    - type: section
      name: "Additional resources"
    - type: subtitle
      name: "Videos"
    - type: link
      name: "Generate a Run in Postman Button | Postman Level Up"
      url: "https://youtu.be/r2DGy4jSuUE"
    - type: link
      name: "WTD: Postman for API development and docs"
      url: "https://podcast.writethedocs.org/2018/01/22/postman-for-api-docs-write-the-docs/"
    - type: subtitle
      name: "Related Blog Posts"
    - type: link
      name: "How to Dynamically Create Custom Environments with Code"
      url: "https://blog.postman.com/how-to-dynamically-create-custom-environments-with-code/"
    - type: section
      name: "Next steps"
    - type: link
      name: "Creating the Run in Postman button"
      url: "/docs/publishing-your-api/run-in-postman/creating-run-button/"

---

The Run in Postman <img alt="Run in Postman button icon" src="https://assets.postman.com/postman-docs/run-in-postman-button-icon.jpg" width="100px"/> button is a way to share a Postman collection (and optional environment) with your users. Live Run in Postman buttons automatically stay updated with changes to your collection, providing consumers with its most recent version. You can also attach an environment to your live button to help onboard new users to your API quickly and efficiently.

## User interaction with your button

When a user comes across the Run in Postman <img alt="Run in Postman button icon" src="https://assets.postman.com/postman-docs/run-in-postman-button-icon.jpg" width="100px"/> button, they can choose to fork the collection to their workspace, view the collection in the public workspace, or import the collection into Postman. Then, they can begin interacting with your API. The Run in Postman button allows the consumers to fork your collection, which creates a copy of the collection while maintaining a link to the parent.

<img alt="Fork collection for run in postman" src="https://assets.postman.com/postman-docs/fork-collection-for-run-in-postman.jpg" height="350px"/>

Run in postman buttons are only available for documentation and embed flows.

> **Security check**: Don't leak sensitive data like access keys in your collection or environment. Read more about [securely using API keys in Postman](https://blog.postman.com/how-to-use-api-keys/).

## Next steps

Create a [Run in Postman button](/docs/publishing-your-api/run-in-postman/creating-run-button/).
