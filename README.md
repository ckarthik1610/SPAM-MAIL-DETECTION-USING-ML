# SPAM-MAIL-DETECTION-USING-ML


{
  "cells": [
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "view-in-github",
        "colab_type": "text"
      },
      "source": [
        "<a href=\"https://colab.research.google.com/github/ckarthik1610/SPAM-MAIL-DETECTION-USING-ML/blob/main/SPAM_MAIL_DETECTION.ipynb\" target=\"_parent\"><img src=\"https://colab.research.google.com/assets/colab-badge.svg\" alt=\"Open In Colab\"/></a>"
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "## **SPAM MAIL DETECTION SYSTEM**"
      ],
      "metadata": {
        "id": "bjN3zO5R13Wn"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "**IMPORTING DEPENDENCIES**"
      ],
      "metadata": {
        "id": "5FtEDK6z3gGD"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "import numpy as np\n",
        "import pandas as pd\n",
        "from sklearn.model_selection import train_test_split\n",
        "from sklearn.feature_extraction.text import TfidfVectorizer\n",
        "from sklearn.linear_model import LogisticRegression\n",
        "from sklearn.metrics import accuracy_score"
      ],
      "metadata": {
        "id": "tJ5j8sj912_1"
      },
      "execution_count": 1,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Accessing the CSV File**"
      ],
      "metadata": {
        "id": "tcnYUSTk3gNY"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "raw_mail_data = pd.read_csv('/content/mail_data.csv')"
      ],
      "metadata": {
        "id": "60FGJclp3bU-"
      },
      "execution_count": 2,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# Raw Data\n",
        "print(raw_mail_data)"
      ],
      "metadata": {
        "id": "bPBUjY1J3tlC",
        "outputId": "caa960d2-aeea-4a25-ac6e-76e86ce3c6e3",
        "colab": {
          "base_uri": "https://localhost:8080/"
        }
      },
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "     Category                                            Message\n",
            "0         ham  Go until jurong point, crazy.. Available only ...\n",
            "1         ham                      Ok lar... Joking wif u oni...\n",
            "2        spam  Free entry in 2 a wkly comp to win FA Cup fina...\n",
            "3         ham  U dun say so early hor... U c already then say...\n",
            "4         ham  Nah I don't think he goes to usf, he lives aro...\n",
            "...       ...                                                ...\n",
            "5567     spam  This is the 2nd time we have tried 2 contact u...\n",
            "5568      ham               Will ü b going to esplanade fr home?\n",
            "5569      ham  Pity, * was in mood for that. So...any other s...\n",
            "5570      ham  The guy did some bitching but I acted like i'd...\n",
            "5571      ham                         Rofl. Its true to its name\n",
            "\n",
            "[5572 rows x 2 columns]\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Filtering Null Datasets**"
      ],
      "metadata": {
        "id": "d4rrSSiT8nkZ"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "mail_data = raw_mail_data.where((pd.notnull(raw_mail_data)),'')\n",
        "mail_data.head(15)"
      ],
      "metadata": {
        "id": "51PXHYlC4RQ7",
        "outputId": "bb2ffab8-8c2b-4e0d-a5b4-3d8929baa324",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 519
        }
      },
      "execution_count": 3,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "   Category                                            Message\n",
              "0       ham  Go until jurong point, crazy.. Available only ...\n",
              "1       ham                      Ok lar... Joking wif u oni...\n",
              "2      spam  Free entry in 2 a wkly comp to win FA Cup fina...\n",
              "3       ham  U dun say so early hor... U c already then say...\n",
              "4       ham  Nah I don't think he goes to usf, he lives aro...\n",
              "5      spam  FreeMsg Hey there darling it's been 3 week's n...\n",
              "6       ham  Even my brother is not like to speak with me. ...\n",
              "7       ham  As per your request 'Melle Melle (Oru Minnamin...\n",
              "8      spam  WINNER!! As a valued network customer you have...\n",
              "9      spam  Had your mobile 11 months or more? U R entitle...\n",
              "10      ham  I'm gonna be home soon and i don't want to tal...\n",
              "11     spam  SIX chances to win CASH! From 100 to 20,000 po...\n",
              "12     spam  URGENT! You have won a 1 week FREE membership ...\n",
              "13      ham  I've been searching for the right words to tha...\n",
              "14      ham                I HAVE A DATE ON SUNDAY WITH WILL!!"
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-c692ad2b-ee19-4108-9f14-5a200450336c\" class=\"colab-df-container\">\n",
              "    <div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>Category</th>\n",
              "      <th>Message</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>ham</td>\n",
              "      <td>Go until jurong point, crazy.. Available only ...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>ham</td>\n",
              "      <td>Ok lar... Joking wif u oni...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>spam</td>\n",
              "      <td>Free entry in 2 a wkly comp to win FA Cup fina...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>ham</td>\n",
              "      <td>U dun say so early hor... U c already then say...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>ham</td>\n",
              "      <td>Nah I don't think he goes to usf, he lives aro...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>5</th>\n",
              "      <td>spam</td>\n",
              "      <td>FreeMsg Hey there darling it's been 3 week's n...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>6</th>\n",
              "      <td>ham</td>\n",
              "      <td>Even my brother is not like to speak with me. ...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>7</th>\n",
              "      <td>ham</td>\n",
              "      <td>As per your request 'Melle Melle (Oru Minnamin...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>8</th>\n",
              "      <td>spam</td>\n",
              "      <td>WINNER!! As a valued network customer you have...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>9</th>\n",
              "      <td>spam</td>\n",
              "      <td>Had your mobile 11 months or more? U R entitle...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>10</th>\n",
              "      <td>ham</td>\n",
              "      <td>I'm gonna be home soon and i don't want to tal...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>11</th>\n",
              "      <td>spam</td>\n",
              "      <td>SIX chances to win CASH! From 100 to 20,000 po...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>12</th>\n",
              "      <td>spam</td>\n",
              "      <td>URGENT! You have won a 1 week FREE membership ...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>13</th>\n",
              "      <td>ham</td>\n",
              "      <td>I've been searching for the right words to tha...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>14</th>\n",
              "      <td>ham</td>\n",
              "      <td>I HAVE A DATE ON SUNDAY WITH WILL!!</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "    <div class=\"colab-df-buttons\">\n",
              "\n",
              "  <div class=\"colab-df-container\">\n",
              "    <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-c692ad2b-ee19-4108-9f14-5a200450336c')\"\n",
              "            title=\"Convert this dataframe to an interactive table.\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\" viewBox=\"0 -960 960 960\">\n",
              "    <path d=\"M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z\"/>\n",
              "  </svg>\n",
              "    </button>\n",
              "\n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    .colab-df-buttons div {\n",
              "      margin-bottom: 4px;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "    <script>\n",
              "      const buttonEl =\n",
              "        document.querySelector('#df-c692ad2b-ee19-4108-9f14-5a200450336c button.colab-df-convert');\n",
              "      buttonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "      async function convertToInteractive(key) {\n",
              "        const element = document.querySelector('#df-c692ad2b-ee19-4108-9f14-5a200450336c');\n",
              "        const dataTable =\n",
              "          await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                    [key], {});\n",
              "        if (!dataTable) return;\n",
              "\n",
              "        const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "          '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "          + ' to learn more about interactive tables.';\n",
              "        element.innerHTML = '';\n",
              "        dataTable['output_type'] = 'display_data';\n",
              "        await google.colab.output.renderOutput(dataTable, element);\n",
              "        const docLink = document.createElement('div');\n",
              "        docLink.innerHTML = docLinkHtml;\n",
              "        element.appendChild(docLink);\n",
              "      }\n",
              "    </script>\n",
              "  </div>\n",
              "\n",
              "\n",
              "<div id=\"df-d32c7ee7-725b-4a36-82b1-77d63cdc54f3\">\n",
              "  <button class=\"colab-df-quickchart\" onclick=\"quickchart('df-d32c7ee7-725b-4a36-82b1-77d63cdc54f3')\"\n",
              "            title=\"Suggest charts\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "<svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\"viewBox=\"0 0 24 24\"\n",
              "     width=\"24px\">\n",
              "    <g>\n",
              "        <path d=\"M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z\"/>\n",
              "    </g>\n",
              "</svg>\n",
              "  </button>\n",
              "\n",
              "<style>\n",
              "  .colab-df-quickchart {\n",
              "      --bg-color: #E8F0FE;\n",
              "      --fill-color: #1967D2;\n",
              "      --hover-bg-color: #E2EBFA;\n",
              "      --hover-fill-color: #174EA6;\n",
              "      --disabled-fill-color: #AAA;\n",
              "      --disabled-bg-color: #DDD;\n",
              "  }\n",
              "\n",
              "  [theme=dark] .colab-df-quickchart {\n",
              "      --bg-color: #3B4455;\n",
              "      --fill-color: #D2E3FC;\n",
              "      --hover-bg-color: #434B5C;\n",
              "      --hover-fill-color: #FFFFFF;\n",
              "      --disabled-bg-color: #3B4455;\n",
              "      --disabled-fill-color: #666;\n",
              "  }\n",
              "\n",
              "  .colab-df-quickchart {\n",
              "    background-color: var(--bg-color);\n",
              "    border: none;\n",
              "    border-radius: 50%;\n",
              "    cursor: pointer;\n",
              "    display: none;\n",
              "    fill: var(--fill-color);\n",
              "    height: 32px;\n",
              "    padding: 0;\n",
              "    width: 32px;\n",
              "  }\n",
              "\n",
              "  .colab-df-quickchart:hover {\n",
              "    background-color: var(--hover-bg-color);\n",
              "    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "    fill: var(--button-hover-fill-color);\n",
              "  }\n",
              "\n",
              "  .colab-df-quickchart-complete:disabled,\n",
              "  .colab-df-quickchart-complete:disabled:hover {\n",
              "    background-color: var(--disabled-bg-color);\n",
              "    fill: var(--disabled-fill-color);\n",
              "    box-shadow: none;\n",
              "  }\n",
              "\n",
              "  .colab-df-spinner {\n",
              "    border: 2px solid var(--fill-color);\n",
              "    border-color: transparent;\n",
              "    border-bottom-color: var(--fill-color);\n",
              "    animation:\n",
              "      spin 1s steps(1) infinite;\n",
              "  }\n",
              "\n",
              "  @keyframes spin {\n",
              "    0% {\n",
              "      border-color: transparent;\n",
              "      border-bottom-color: var(--fill-color);\n",
              "      border-left-color: var(--fill-color);\n",
              "    }\n",
              "    20% {\n",
              "      border-color: transparent;\n",
              "      border-left-color: var(--fill-color);\n",
              "      border-top-color: var(--fill-color);\n",
              "    }\n",
              "    30% {\n",
              "      border-color: transparent;\n",
              "      border-left-color: var(--fill-color);\n",
              "      border-top-color: var(--fill-color);\n",
              "      border-right-color: var(--fill-color);\n",
              "    }\n",
              "    40% {\n",
              "      border-color: transparent;\n",
              "      border-right-color: var(--fill-color);\n",
              "      border-top-color: var(--fill-color);\n",
              "    }\n",
              "    60% {\n",
              "      border-color: transparent;\n",
              "      border-right-color: var(--fill-color);\n",
              "    }\n",
              "    80% {\n",
              "      border-color: transparent;\n",
              "      border-right-color: var(--fill-color);\n",
              "      border-bottom-color: var(--fill-color);\n",
              "    }\n",
              "    90% {\n",
              "      border-color: transparent;\n",
              "      border-bottom-color: var(--fill-color);\n",
              "    }\n",
              "  }\n",
              "</style>\n",
              "\n",
              "  <script>\n",
              "    async function quickchart(key) {\n",
              "      const quickchartButtonEl =\n",
              "        document.querySelector('#' + key + ' button');\n",
              "      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.\n",
              "      quickchartButtonEl.classList.add('colab-df-spinner');\n",
              "      try {\n",
              "        const charts = await google.colab.kernel.invokeFunction(\n",
              "            'suggestCharts', [key], {});\n",
              "      } catch (error) {\n",
              "        console.error('Error during call to suggestCharts:', error);\n",
              "      }\n",
              "      quickchartButtonEl.classList.remove('colab-df-spinner');\n",
              "      quickchartButtonEl.classList.add('colab-df-quickchart-complete');\n",
              "    }\n",
              "    (() => {\n",
              "      let quickchartButtonEl =\n",
              "        document.querySelector('#df-d32c7ee7-725b-4a36-82b1-77d63cdc54f3 button');\n",
              "      quickchartButtonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "    })();\n",
              "  </script>\n",
              "</div>\n",
              "\n",
              "    </div>\n",
              "  </div>\n"
            ],
            "application/vnd.google.colaboratory.intrinsic+json": {
              "type": "dataframe",
              "variable_name": "mail_data",
              "summary": "{\n  \"name\": \"mail_data\",\n  \"rows\": 5572,\n  \"fields\": [\n    {\n      \"column\": \"Category\",\n      \"properties\": {\n        \"dtype\": \"category\",\n        \"num_unique_values\": 2,\n        \"samples\": [\n          \"spam\",\n          \"ham\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Message\",\n      \"properties\": {\n        \"dtype\": \"string\",\n        \"num_unique_values\": 5157,\n        \"samples\": [\n          \"Also sir, i sent you an email about how to log into the usc payment portal. I.ll send you another message that should explain how things are back home. Have a great weekend.\",\n          \"Are you free now?can i call now?\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}"
            }
          },
          "metadata": {},
          "execution_count": 3
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "#Remaining Datasets after filtering Null datasets out\n",
        "mail_data.shape"
      ],
      "metadata": {
        "id": "4uoYh2Z58z9k"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Assigning Labels as 0 and 1**"
      ],
      "metadata": {
        "id": "EQ42OSDK4SXE"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "mail_data.loc[mail_data['Category'] == 'spam', 'Category',] = 0\n",
        "mail_data.loc[mail_data['Category'] == 'ham', 'Category',] = 1"
      ],
      "metadata": {
        "id": "t7QPwMip47x-"
      },
      "execution_count": 4,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "#Seperating data as text and Label\n",
        "X = mail_data['Message']\n",
        "Y = mail_data['Category']"
      ],
      "metadata": {
        "id": "1Vq8TwHg5L-m"
      },
      "execution_count": 5,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Splitting Training and Testing Data (80/20)**"
      ],
      "metadata": {
        "id": "ZMZdWry65cif"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.2, random_state=5)"
      ],
      "metadata": {
        "id": "G4qXXpN45oFv"
      },
      "execution_count": 6,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "#Split\n",
        "print(X.shape)\n",
        "print(X_train.shape)\n",
        "print(X_test.shape)"
      ],
      "metadata": {
        "id": "cRD-edtH5192",
        "outputId": "538e0f19-c906-472d-fb48-9d332217ba45",
        "colab": {
          "base_uri": "https://localhost:8080/"
        }
      },
      "execution_count": 7,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "(5572,)\n",
            "(4457,)\n",
            "(1115,)\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Extracting key features from the Text that can help differentiate a SPAM mail or a HAM mail**"
      ],
      "metadata": {
        "id": "h-tcv3H358au"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "# transform the text data to feature vectors that can be used as input to the Logistic regression\n",
        "\n",
        "feature_extraction = TfidfVectorizer(min_df = 1, stop_words='english', lowercase=True)\n",
        "\n",
        "X_train_features = feature_extraction.fit_transform(X_train)\n",
        "X_test_features = feature_extraction.transform(X_test)\n",
        "\n",
        "# convert Y_train and Y_test values as integers\n",
        "\n",
        "Y_train = Y_train.astype('int')\n",
        "Y_test = Y_test.astype('int')"
      ],
      "metadata": {
        "id": "urj6a_Db58ug"
      },
      "execution_count": 8,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Displaying the numerical representation of the features in the training dataset**"
      ],
      "metadata": {
        "id": "2G_DQxmM98P4"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "print(X_train)"
      ],
      "metadata": {
        "id": "XySmPerw-czm"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "print(X_train_features)"
      ],
      "metadata": {
        "id": "2ufhm-0I-8vG"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "**TRAINING THE LOGISTIC REGRESSION MODEL**"
      ],
      "metadata": {
        "id": "QPu8_gX__BSf"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "model = LogisticRegression()\n",
        "model.fit(X_train_features, Y_train)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 74
        },
        "id": "gRGzPme4_Dzs",
        "outputId": "f4e3d7fb-b80f-4572-810d-e7e066cb8a85"
      },
      "execution_count": 10,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "LogisticRegression()"
            ],
            "text/html": [
              "<style>#sk-container-id-1 {color: black;background-color: white;}#sk-container-id-1 pre{padding: 0;}#sk-container-id-1 div.sk-toggleable {background-color: white;}#sk-container-id-1 label.sk-toggleable__label {cursor: pointer;display: block;width: 100%;margin-bottom: 0;padding: 0.3em;box-sizing: border-box;text-align: center;}#sk-container-id-1 label.sk-toggleable__label-arrow:before {content: \"▸\";float: left;margin-right: 0.25em;color: #696969;}#sk-container-id-1 label.sk-toggleable__label-arrow:hover:before {color: black;}#sk-container-id-1 div.sk-estimator:hover label.sk-toggleable__label-arrow:before {color: black;}#sk-container-id-1 div.sk-toggleable__content {max-height: 0;max-width: 0;overflow: hidden;text-align: left;background-color: #f0f8ff;}#sk-container-id-1 div.sk-toggleable__content pre {margin: 0.2em;color: black;border-radius: 0.25em;background-color: #f0f8ff;}#sk-container-id-1 input.sk-toggleable__control:checked~div.sk-toggleable__content {max-height: 200px;max-width: 100%;overflow: auto;}#sk-container-id-1 input.sk-toggleable__control:checked~label.sk-toggleable__label-arrow:before {content: \"▾\";}#sk-container-id-1 div.sk-estimator input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-1 div.sk-label input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-1 input.sk-hidden--visually {border: 0;clip: rect(1px 1px 1px 1px);clip: rect(1px, 1px, 1px, 1px);height: 1px;margin: -1px;overflow: hidden;padding: 0;position: absolute;width: 1px;}#sk-container-id-1 div.sk-estimator {font-family: monospace;background-color: #f0f8ff;border: 1px dotted black;border-radius: 0.25em;box-sizing: border-box;margin-bottom: 0.5em;}#sk-container-id-1 div.sk-estimator:hover {background-color: #d4ebff;}#sk-container-id-1 div.sk-parallel-item::after {content: \"\";width: 100%;border-bottom: 1px solid gray;flex-grow: 1;}#sk-container-id-1 div.sk-label:hover label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-1 div.sk-serial::before {content: \"\";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: 0;}#sk-container-id-1 div.sk-serial {display: flex;flex-direction: column;align-items: center;background-color: white;padding-right: 0.2em;padding-left: 0.2em;position: relative;}#sk-container-id-1 div.sk-item {position: relative;z-index: 1;}#sk-container-id-1 div.sk-parallel {display: flex;align-items: stretch;justify-content: center;background-color: white;position: relative;}#sk-container-id-1 div.sk-item::before, #sk-container-id-1 div.sk-parallel-item::before {content: \"\";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: -1;}#sk-container-id-1 div.sk-parallel-item {display: flex;flex-direction: column;z-index: 1;position: relative;background-color: white;}#sk-container-id-1 div.sk-parallel-item:first-child::after {align-self: flex-end;width: 50%;}#sk-container-id-1 div.sk-parallel-item:last-child::after {align-self: flex-start;width: 50%;}#sk-container-id-1 div.sk-parallel-item:only-child::after {width: 0;}#sk-container-id-1 div.sk-dashed-wrapped {border: 1px dashed gray;margin: 0 0.4em 0.5em 0.4em;box-sizing: border-box;padding-bottom: 0.4em;background-color: white;}#sk-container-id-1 div.sk-label label {font-family: monospace;font-weight: bold;display: inline-block;line-height: 1.2em;}#sk-container-id-1 div.sk-label-container {text-align: center;}#sk-container-id-1 div.sk-container {/* jupyter's `normalize.less` sets `[hidden] { display: none; }` but bootstrap.min.css set `[hidden] { display: none !important; }` so we also need the `!important` here to be able to override the default hidden behavior on the sphinx rendered scikit-learn.org. See: https://github.com/scikit-learn/scikit-learn/issues/21755 */display: inline-block !important;position: relative;}#sk-container-id-1 div.sk-text-repr-fallback {display: none;}</style><div id=\"sk-container-id-1\" class=\"sk-top-container\"><div class=\"sk-text-repr-fallback\"><pre>LogisticRegression()</pre><b>In a Jupyter environment, please rerun this cell to show the HTML representation or trust the notebook. <br />On GitHub, the HTML representation is unable to render, please try loading this page with nbviewer.org.</b></div><div class=\"sk-container\" hidden><div class=\"sk-item\"><div class=\"sk-estimator sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-1\" type=\"checkbox\" checked><label for=\"sk-estimator-id-1\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">LogisticRegression</label><div class=\"sk-toggleable__content\"><pre>LogisticRegression()</pre></div></div></div></div></div>"
            ]
          },
          "metadata": {},
          "execution_count": 10
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "**EVALUATING THE MODEL**\n",
        "\n"
      ],
      "metadata": {
        "id": "Mx_YOmLh_7Yt"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "Training Data"
      ],
      "metadata": {
        "id": "Gfg7fFixBQpx"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "# Prediction\n",
        "prediction_on_training_data = model.predict(X_train_features)\n",
        "accuracy_on_training_data = accuracy_score(Y_train, prediction_on_training_data)"
      ],
      "metadata": {
        "id": "zUI-ljY1_9sN"
      },
      "execution_count": 11,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# Accuracy\n",
        "print('Accuracy on training data : ', accuracy_on_training_data)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "UfquTseBA9EQ",
        "outputId": "1004981e-2541-4368-95dd-ed15aaede82d"
      },
      "execution_count": 12,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Accuracy on training data :  0.9676912721561588\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "Testing Data"
      ],
      "metadata": {
        "id": "Qf8DnwD5Cc0X"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "# Prediction\n",
        "prediction_on_test_data = model.predict(X_test_features)\n",
        "accuracy_on_test_data = accuracy_score(Y_test, prediction_on_test_data)"
      ],
      "metadata": {
        "id": "Pi9mDF3QBKs2"
      },
      "execution_count": 13,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# Accuracy\n",
        "print('Accuracy on test data : ', accuracy_on_test_data)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "5RBqiOkoBK-I",
        "outputId": "6e2243e7-3711-4ed5-b5ec-3a37580ba516"
      },
      "execution_count": 14,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Accuracy on test data :  0.9605381165919282\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "**PREDICTION SYSTEM**"
      ],
      "metadata": {
        "id": "tWpILMVNDBix"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "input_mail = [\"I'm gonna be home soon and i don't want to talk about this stuff anymore tonight, k? I've cried enough today.\"]\n",
        "\n",
        "# convert text to feature vectors\n",
        "input_data_features = feature_extraction.transform(input_mail)\n",
        "\n",
        "# making prediction\n",
        "\n",
        "prediction = model.predict(input_data_features)\n",
        "print(prediction)\n",
        "\n",
        "\n",
        "if (prediction[0]==1):\n",
        "  print('Ham mail')\n",
        "\n",
        "else:\n",
        "  print('Spam mail')"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "qHATL2MMDFC8",
        "outputId": "21a792ed-0cdf-451d-f038-3db56e65675e"
      },
      "execution_count": 15,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "[1]\n",
            "Ham mail\n"
          ]
        }
      ]
    }
  ],
  "metadata": {
    "colab": {
      "provenance": [],
      "include_colab_link": true
    },
    "google": {
      "image_path": "/static/site-assets/images/docs/logo-python.svg",
      "keywords": [
        "examples",
        "gemini",
        "beginner",
        "googleai",
        "quickstart",
        "python",
        "text",
        "chat",
        "vision",
        "embed"
      ]
    },
    "kernelspec": {
      "display_name": "Python 3",
      "name": "python3"
    }
  },
  "nbformat": 4,
  "nbformat_minor": 0
}
