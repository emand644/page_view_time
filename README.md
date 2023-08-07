# page_view_time
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

def draw_line_plot():
    # Import data and set index
    df = pd.read_csv('fcc-forum-pageviews.csv', parse_dates=['date'], index_col='date')
    
    # Clean data
    df_cleaned = df[
        (df['value'] >= df['value'].quantile(0.025)) &
        (df['value'] <= df['value'].quantile(0.975))
    ]
    
    # Create line plot
    plt.figure(figsize=(10, 5))
    plt.plot(df_cleaned.index, df_cleaned['value'], color='r')
    plt.title('Daily freeCodeCamp Forum Page Views 5/2016-12/2019')
    plt.xlabel('Date')
    plt.ylabel('Page Views')
    plt.show()

def draw_bar_plot():
    # Import data and set index
    df = pd.read_csv('fcc-forum-pageviews.csv', parse_dates=['date'], index_col='date')
    
    # Clean data
    df_cleaned = df[
        (df['value'] >= df['value'].quantile(0.025)) &
        (df['value'] <= df['value'].quantile(0.975))
    ]
    
    # Create year and month columns
    df_cleaned['year'] = df_cleaned.index.year
    df_cleaned['month'] = df_cleaned.index.month_name()
    
    # Create bar plot
    df_bar = df_cleaned.groupby(['year', 'month'])['value'].mean().unstack()
    ax = df_bar.plot(kind='bar', figsize=(10, 6))
    plt.xlabel('Years')
    plt.ylabel('Average Page Views')
    plt.legend(title='Months')
    plt.show()

def draw_box_plot():
    # Import data and set index
    df = pd.read_csv('fcc-forum-pageviews.csv', parse_dates=['date'], index_col='date')
    
    # Create year and month columns
    df['year'] = df.index.year
    df['month'] = df.index.month_name()
    
    # Create subplots
    fig, axes = plt.subplots(1, 2, figsize=(20, 8))
    
    # Year-wise box plot
    sns.boxplot(x='year', y='value', data=df, ax=axes[0])
    axes[0].set_title('Year-wise Box Plot (Trend)')
    axes[0].set_xlabel('Year')
    axes[0].set_ylabel('Page Views')
    
    # Month-wise box plot
    month_order = ['January', 'February', 'March', 'April', 'May', 'June', 'July',
                   'August', 'September', 'October', 'November', 'December']
    sns.boxplot(x='month', y='value', data=df, ax=axes[1], order=month_order)
    axes[1].set_title('Month-wise Box Plot (Seasonality)')
    axes[1].set_xlabel('Month')
    axes[1].set_ylabel('Page Views')
    
    plt.tight_layout()
    plt.show()

# Call the functions to generate the plots
draw_line_plot()
draw_bar_plot()
draw_box_plot()
