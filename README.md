```
#import "ViewController.h"

@interface ViewController ()<UITextViewDelegate>

@property (strong, nonatomic) UITextView *contentTxtView;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    [self setupUI];
    [self setContent];
}

- (void) setupUI {
    self.contentTxtView = [[UITextView alloc]init];
    self.contentTxtView.delegate = self;
    self.contentTxtView.editable = false;
    self.contentTxtView.scrollEnabled = true;
    self.contentTxtView.linkTextAttributes = @{NSForegroundColorAttributeName:[UIColor redColor]};
    self.contentTxtView.translatesAutoresizingMaskIntoConstraints = NO;
    [self.view addSubview:self.contentTxtView];
    
    [[self.contentTxtView.centerXAnchor constraintEqualToAnchor:self.view.centerXAnchor] setActive:YES];
    [[self.contentTxtView.centerYAnchor constraintEqualToAnchor:self.view.centerYAnchor] setActive:YES];
    [[self.contentTxtView.widthAnchor constraintEqualToConstant:180] setActive:YES];
    [[self.contentTxtView.heightAnchor constraintEqualToConstant:200] setActive:YES];
}

- (void)setContent{
    NSString *content = @"《用户服务协议》和《隐私政策》";
     NSMutableAttributedString *attStr = [[NSMutableAttributedString alloc]initWithString:content];
    
    // 设置行间距
    NSMutableParagraphStyle *paragraphStyle = [[NSMutableParagraphStyle alloc] init];
    [paragraphStyle setLineSpacing:5];
    [attStr addAttribute:NSParagraphStyleAttributeName value:paragraphStyle range:NSMakeRange(0, [content length])];
    
    NSArray *titiles = @[@"《用户服务协议》", @"《隐私政策》"];
    for (int i = 0; i < [titiles count]; i++) {
        NSRange range = [content rangeOfString:titiles[i]];
        [attStr addAttribute:NSLinkAttributeName value:@"click://" range:NSMakeRange(range.location, range.length)];
        [attStr addAttributes:@{NSFontAttributeName: [UIFont systemFontOfSize:15]} range:NSMakeRange(range.location, range.length)];

    }
    self.contentTxtView.attributedText = attStr;
    
    /*
    NSMutableAttributedString *attStr = [[NSMutableAttributedString alloc]initWithString:content];
    [attStr addAttribute:NSLinkAttributeName value:@"click://" range:NSMakeRange(0, 8)];
    [attStr addAttribute:NSLinkAttributeName value:@"click1://" range:NSMakeRange(9, 6)];
    [attStr addAttributes:@{NSFontAttributeName: [UIFont systemFontOfSize:15]} range:NSMakeRange(0, 8)];
    [attStr addAttributes:@{NSFontAttributeName: [UIFont systemFontOfSize:15]} range:NSMakeRange(9, 6)];
     */
}

- (BOOL)textView:(UITextView *)textView shouldInteractWithURL:(NSURL *)URL inRange:(NSRange)characterRange {
    if ([[URL scheme] isEqualToString:@"click"]) {
        NSAttributedString *abStr = [textView.attributedText attributedSubstringFromRange:characterRange];
        NSLog(@"%@", abStr);
        return NO;
    }
    if ([[URL scheme] isEqualToString:@"click1"]) {
        NSAttributedString *abStr = [textView.attributedText attributedSubstringFromRange:characterRange];
        NSLog(@"%@", abStr);
        return NO;
    }
    return YES;
}


@end
```
